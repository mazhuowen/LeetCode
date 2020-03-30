[toc]

A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and empty space is marked as 1 and 0 respectively in the grid.

Note: m and n will be at most 100.



## 题目解读



```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {

    }
}
```

## 程序设计

* 与[#62 Unique Paths](./#62 Unique Paths.md)一致，只是在有障碍点的位置路径需要清零。

```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        if (obstacleGrid == null || obstacleGrid.length == 0 || obstacleGrid[0][0] == 1
                || obstacleGrid[obstacleGrid.length - 1][obstacleGrid[0].length - 1] == 1) return 0;

        int m = obstacleGrid.length, n = obstacleGrid[0].length;
        int[] dp = new int[n];
        dp[0] = 1;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // 有障碍，路径清零
                if (obstacleGrid[i][j] == 1) {
                    dp[j] = 0;
                } else if (i == 0 && j > 0) {
                    dp[j] = dp[j - 1];
                } else if (i > 0 && j > 0) {
                    dp[j] += dp[j - 1];
                }
            }
        }
        return dp[n - 1];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.2MB，在所有java提交中击败了46.65%的用户。

## 官方解题

&emsp;官方直接在矩形上做动态规划，其它思路一致。