[toc]

There are n cities numbered from $1$ to $n$. You are given an array `edges` of size $n-1$, where `edges[i] = [ui, vi]` represents a bidirectional edge between cities `ui` and `vi`. There exists a unique path between each pair of cities. In other words, the cities form a **tree**.

A **subtree** is a subset of cities where every city is reachable from every other city in the subset, where the path between each pair passes through only the cities from the subset. Two subtrees are different if there is a city in one subtree that is not present in the other.

For each `d` from $1$ to $n-1$, find the number of subtrees in which the **maximum distance** between any two cities in the subtree is equal to `d​`.

Return an array of size $n-1$ where the `d`th element (**1-indexed**) is the number of subtrees in which the **maximum distance** between any two cities is equal to `d`.

**Notice** that the **distance** between the two cities is the number of edges in the path between them.



**Constraints**:

* $2 \le n \le 15$
* $\text{edges.length} == n-1$
* $\text{edges[i].length} == 2$
* $1 \le \text{ui, vi} \le n$
* All pairs `(ui, vi)` are distinct.



## 题目解读

&emsp;统计所有子树结构的最大距离，并返回各个距离子树的数目。

```java
class Solution {
    public int[] countSubgraphsForEachDiameter(int n, int[][] edges) {

    }
}
```

## 程序设计

* 参考社区思路，采用`Floyd`提前计算顶点对之间的距离，然后采用状压动态规划计算子树最大距离，最后统计生成答案。

```java
class Solution {
    public int[] countSubgraphsForEachDiameter(int n, int[][] edges) {
        // 状压动态规划
        int[] dp = new int[1 << n];
        int[][] dis = new int[n][n];
        // 初始化
        for (int i = 0; i < n; i++) {
            Arrays.fill(dis[i], Integer.MAX_VALUE / 2);
            dis[i][i] = 0;
        }
        for (int[] edge : edges) {
            int v1 = edge[0] - 1, v2 = edge[1] - 1;
            dis[v1][v2] = dis[v2][v1] = 1;
            dp[(1 << v1) | (1 << v2)] = 1;
        }
        // 计算距离
        floyd(dis, n);

        for (int stat = 1; stat < (1 << n); stat++) {
            if (dp[stat] == 0) continue;
            for (int i = 0; i < n; i++) {
                // 已经包含在子树中或与子树不相连或者已经计算过
                if ((stat & (1 << i)) != 0 || !isCollect(stat, i, dis) || dp[stat | (1 << i)] != 0) continue;
                dp[stat | (1 << i)] = Math.max(dp[stat], getMaxDis(stat, i, dis));
            }
        }

        int[] res = new int[n - 1];
        for (int i = 1; i < (1 << n); i++) {
            if (dp[i] != 0) res[dp[i] - 1]++;
        }
        return res;
    }

    private void floyd(int[][] dis, int n) {
        // Floyd算法
        for (int k = 0; k < n; k++) {
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++) {
                    if (dis[i][j] > dis[i][k] + dis[k][j]) {
                        dis[i][j] = dis[i][k] + dis[k][j];
                    }
                }
            }
        }
    }

    private boolean isCollect(int stat, int next, int[][] dis) {
        for (int i = 0; i < dis.length && stat > 0; stat >>= 1, i++) {
            // 存在直接相连边
            if ((stat & 1) == 1 && dis[i][next] == 1) return true;
        }
        return false;
    }

    private int getMaxDis(int stat, int next, int[][] dis) {
        int max = 0;
        for (int i = 0; i < dis.length && stat > 0; stat >>= 1, i++) {
            if ((stat & 1) == 1) max = Math.max(max, dis[i][next]);
        }
        return max;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(V2^V)$，空间复杂度为$O(2^V)$。



## 官方解题

&emsp;