[toc]

There are `N` cities numbered from `1` to `N`.

You are given `connections`, where each `connections[i] = [city1, city2, cost]` represents the cost to connect `city1` and `city2` together.  (A connection is bidirectional: connecting `city1` and `city2` is the same as connecting `city2` and `city1`.)

Return the minimum cost so that for every pair of cities, there exists a path of connections (possibly of length 1) that connects those two cities together.  The cost is the sum of the connection costs used. If the task is impossible, return -1.



Note:

* $1 \le N \le 10000$
* $1 \le \text{connections.length} \le 10000$
* $1 \le \text{connections[i][0], connections[i][1]} \le N$
* $0 \le \text{connections[i][2]} \le 10^5$
* $\text{connections[i][0]} \ne \text{connections[i][1]}$



## 题目解读

&emsp;最小生成树基本问题。

```java
class Solution {
    public int minimumCost(int N, int[][] connections) {

    }
}
```

## 程序设计

* 最小生成树问题，采用`Kruskal`算法，以边的角度贪心搜索。

```java
class Solution {
    public int minimumCost(int N, int[][] connections) {
        // 无合法或组不成树
        if (N <= 0 || connections.length < N - 1) return -1;
        // 优先级队列，根据边的权重排序
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> a[2] - b[2]);
        for (int[] connection : connections) {
            queue.add(connection);
        }
        // 不相交集
        DisJoint disJoint = new DisJoint(N);
        // 接收的边，成本
        int accept = 0, weight = 0;
        while (accept < N - 1 && !queue.isEmpty()) {
            int[] cur = queue.poll();
            if (disJoint.find(cur[0] - 1) != disJoint.find(cur[1] - 1)) {
                disJoint.union(disJoint.find(cur[0] - 1), disJoint.find(cur[1] - 1));
                weight += cur[2];
                accept++;
            }
        }
        // 判断是否连通
        return disJoint.size() == 1 ? weight : -1;
    }
}

class DisJoint {
    private int[] tree;
    private int size;

    DisJoint(int size) {
        this.size = size;
        this.tree = new int[size];
        Arrays.fill(tree, -1);
    }

    public void union(int root1, int root2) {
        if (tree[root1] >= 0 || tree[root2] >= 0) throw new IllegalArgumentException("invalid param");
        if (root1 == root2) return;

        if (tree[root1] <= tree[root2]) {
            tree[root1] += tree[root2];
            tree[root2] = root1;
        } else {
            tree[root2] += tree[root1];
            tree[root1] = root2;
        }
        size--;
    }

    public int find(int idx) {
        if (tree[idx] < 0) return idx;
        return tree[idx] = find(tree[idx]);
    }

    public int size() {
        return size;
    }
}
```

## 性能分析

&emsp;时间复杂度，入队$O(E\log_2E)$，贪心搜索$O(V\log_2E)$，在本题中$V < E$，总的时间复杂度为$O(E\log_2E)$；空间复杂度为$O(E)$。

执行用时：24ms，在所有java提交中击败了96.15%的用户。

内存消耗：47.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方解题还提供了`Prim`算法的思路，但该题适合使用`Kruskal`。