[toc]

There are $n$ houses in a village. We want to supply water for all the houses by building wells and laying pipes.

For each house $i$, we can either build a well inside it directly with cost `wells[i]`, or pipe in water from another well to it. The costs to lay pipes between houses are given by the array pipes, where each `pipes[i] = [house1, house2, cost]` represents the cost to connect `house1` and `house2` together using a pipe. Connections are bidirectional.

Find the minimum total cost to supply water to all houses.



Constraints:

* $1 \le n \le 10000$
* $\text{wells.length} == n$
* $0 \le \text{wells[i]} \le 10^5$
* $1 \le \text{pipes.length} \le 10000$
* $1 \le \text{pipes[i][0], pipes[i][1]} \le n$
* $0 \le \text{pipes[i][2]} \le 10^5$
* $\text{pipes[i][0]} \ne \text{pipes[i][1]}$



## 题目解读

&emsp;村里面一共有$$栋房子，希望通过建造水井和铺设管道来为所有房子供水。对于每个房子`i`，有两种可选的供水方案：

* 一种是直接在房子内建造水井，成本为`wells[i]`；
* 另一种是从另一口井铺设管道引水，数组`pipes`给出了在房子间铺设管道的成本，其中每个`pipes[i] = [house1, house2, cost]`代表用管道将`house1`和`house2`连接在一起的成本。当然连接是双向的。

计算为所有房子都供水的最低总成本。

```java
class Solution {
    public int minCostToSupplyWater(int n, int[] wells, int[][] pipes) {

    }
}
```

## 程序设计

* 首先想到的是生成最小树问题，每个连通分量对应一个最小生成树，即管道铺设方案。每个连通分量只需选择分量内部开通水井花费最小的，这样得到最后的铺设方案。
* 首先不相交集需要变化，需要额外记录每个不相交集的最小水井花费，变化如下：

```java
class DisJoint {
    // 带权重的不相交集，位置0表示根，位置1表示本集合最小的井的花费
    private int[][] tree;
    private int size;

    DisJoint(int size, int[] weights) {
        this.size = size;
        this.tree = new int[size][2];
        for (int i = 0; i < size; i++) {
            tree[i][0] = -1;
            // 初始化权重为每个房子需要的水井开销
            tree[i][1] = weights[i];
        }
    }

    public void union(int root1, int root2) {
        if (tree[root1][0] >= 0 || tree[root2][0] >= 0) throw new IllegalArgumentException("invalid param");
        if (root1 == root2) return;
		
        // 合并两个集合并更新新的根节点所在集合最小水井开销
        if (tree[root1][0] <= tree[root2][0]) {
            tree[root1][0] += tree[root2][0];
            tree[root2][0] = root1;
            tree[root1][1] = Math.min(tree[root1][1], tree[root2][1]);
        } else {
            tree[root2][0] += tree[root1][0];
            tree[root1][0] = root2;
            tree[root2][1] = Math.min(tree[root1][1], tree[root2][1]);
        }
        size--;
    }

    public int find(int idx) {
        if (tree[idx][0] < 0) return idx;
        return tree[idx][0] = find(tree[idx][0]);
    }

    public int size() {
        return size;
    }

    // 返回所查集合水井的最小开销
    public int getMinWeight(int idx) {
        if (tree[idx][0] < 0) return tree[idx][1];
        return getMinWeight(tree[idx][0]);
    }
}
```

* 得到不相交集后，主体代码如下，相比`Kruskal`算法只是最后增加了遍历各个集合，添加开通水井的花销：

```java
class Solution {
    public int minCostToSupplyWater(int n, int[] wells, int[][] pipes) {
        if (n <= 0) return 0;
        // 权重队列
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> a[2] - b[2]);
        for (int[] pipe : pipes) {
            queue.add(pipe);
        }
        DisJoint disJoint = new DisJoint(n, wells);
        int accept = 0;
        int weight = 0;
        while (accept < n - 1 && !queue.isEmpty()) {
            int[] cur = queue.poll();
            if (disJoint.find(cur[0] - 1) != disJoint.find(cur[1] - 1)) {
                disJoint.union(disJoint.find(cur[0] - 1), disJoint.find(cur[1] - 1));
                weight += cur[2];
                accept++;
            }
        }

        boolean[] flag = new boolean[n];
        for (int i = 0; i < n; i++) {
            int root = disJoint.find(i);
            if (flag[root]) continue;
            weight += disJoint.getMinWeight(root);
            flag[root] = true;
        }
        return weight;
    }
}
```

* 上述思路只考虑了最小生成树，没有考虑开通水井和铺设管道花费的对比，测试用例不通过。事实上当两个房子之间如果不铺设管道，需要在两个房子所在的集合开通水井，记为`s1`、`s2`；当两个房子连通，则只需开通一个水井，这个水井是`s1`、`s2`中的较小值，并需要铺设管道，花销记为`s3`。则只有当`s3`小于等于`s1`、`s2`中的较大值时，铺设管道才划算，否则即使可以铺设，各自断开开通水井更划算。
* 有了上述思路在`Kruskal`的基础上做改动，不相交集沿用前面的结构。

```java
class Solution {
    public int minCostToSupplyWater(int n, int[] wells, int[][] pipes) {
        if (n <= 0) return 0;
        // 权重队列
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> a[2] - b[2]);
        for (int[] pipe : pipes) {
            queue.add(pipe);
        }
        DisJoint disJoint = new DisJoint(n, wells);
        int accept = 0;
        int weight = 0;
        while (accept < n - 1 && !queue.isEmpty()) {
            int[] cur = queue.poll();
            int root1 = disJoint.find(cur[0] - 1);
            int root2 = disJoint.find(cur[1] - 1);
            // 不形成环路同时铺设管道花销小于等于开通较大的水井的情况下才会铺设
            if (root1 != root2 && Math.max(disJoint.getMinWeight(root1), disJoint.getMinWeight(root2)) >= cur[2]) {
                disJoint.union(root1, root2);
                weight += cur[2];
                accept++;
            }
        }

        // 遍历加入每个集合最小水井开通花销
        boolean[] flag = new boolean[n];
        for (int i = 0; i < n; i++) {
            int root = disJoint.find(i);
            if (flag[root]) continue;
            weight += disJoint.getMinWeight(root);
            flag[root] = true;
        }
        return weight;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\max(E\log_2E,V\log_2E)$，空间复杂度为$O(\max(V,E))$。

执行用时：28ms，在所有java提交中击败了72.55%的用户。

内存消耗：43.5MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。