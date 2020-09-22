[toc]

You are given two groups of points where the first group has `size1` points, the second group has `size2` points, and $\text{size}_1 \ge \text{size}_2$.

The `cost` of the connection between any two points are given in an $\text{size}_1 \times \text{size}_2$ matrix where `cost[i][j]` is the cost of connecting point $i$ of the first group and point $j$ of the second group. The groups are connected if **each point in both groups is connected to one or more points in the opposite group**. In other words, each point in the first group must be connected to at least one point in the second group, and each point in the second group must be connected to at least one point in the first group.

Return the minimum cost it takes to connect the two groups.



**Constraints**:

* $\text{size}_1 == \text{cost.length}$
* $\text{size}_2 == \text{cost[i].length}$
* $1 \le \text{size}_1, \text{size}_2 \le 12$
* $\text{size}_1 \ge \text{size}_2$
* $0 \le \text{cost[i][j]} \le 100$



## 题目解读

&emsp;给定两组点，第一组点大于等于第二组点，现希望每组点都与另一组点相连，求最小权重。

```java
class Solution {
    public int connectTwoGroups(List<List<Integer>> cost) {

    }
}
```

## 程序设计

* 参考社区思路，采用状态压缩动态规划`dp(i,j)`表示选择到第一组第$i$个节点时，第二组的已选节点压缩状态为$j$的最小代价。

```java
class Solution {
    public int connectTwoGroups(List<List<Integer>> cost) {
        int n = cost.size(), m = cost.get(0).size(), stat = 1 << m;
        // allCost[i][j]表示在i点选择连接状态j所需代价
        int[][] allCost = new int[n][stat];
        for (int i = 0; i < n; i++) {
            for (int j = 1; j < stat; j++) {
                int c = 0;
                for (int k = 0; k < m; k++) if (((j >> k) & 1) == 1) c += cost.get(i).get(k);
                allCost[i][j] = c; 
            }
        }

        // 动态规划，dp(i,j)表示在第i个节点已连接状态为j的最小代价
        int[][] dp = new int[n][stat];
        for (int[] array : dp) Arrays.fill(array, Integer.MAX_VALUE);
        dp[0] = allCost[0];

        for (int i = 1; i < n; i++) {
            // 当前节点选择连接第二组节点状态
            for (int j = 1; j < stat; j++) {
                // 与之前状态合并对比并更新
                for (int k = 1; k < stat; k++) {
                    dp[i][j | k] = Math.min(dp[i][j | k], dp[i - 1][k] + allCost[i][j]);
                }
            }
        }
        return dp[n - 1][stat - 1];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM2^M)$，空间复杂度为$O(N2^M)$。



## 官方解题

&emsp;