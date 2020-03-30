[toc]

Given a $m \times n$ grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

Note: You can only move either down or right at any point in time.



## 题目解读

&emsp;给定非负矩阵，找到从左上到右下的路径的最小和。题目限定只能右移或下移。

```java
class Solution {
    public int minPathSum(int[][] grid) {

    }
}
```

## 程序设计

* 仔细分析，该问题可以分解为一系列子问题，可以得到递推方程为$dp(i,j) = grid(i,j) + min(dp(i + 1,j), dp(i,j+1))$。采用动态规划，二维数组保存当前位置的最小路径和。

```java
class Solution {
    public int minPathSum(int[][] grid) {
        if (grid == null || grid.length == 0) return 0;

        // 保存当前位置最小路径和
        int[][] dp = new int[grid.length][grid[0].length];
        dp[0][0] = grid[0][0];

        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (i == 0 && j == 0) continue;
                // 根据上面和左边元素更新当前位置
                dp[i][j] = Integer.MAX_VALUE;
                if (i - 1 >= 0) {
                    dp[i][j] = Math.min(dp[i][j], dp[i - 1][j] + grid[i][j]);
                }
                if (j - 1 >= 0) {
                    dp[i][j] = Math.min(dp[i][j], dp[i][j - 1] + grid[i][j]);
                }
            }
        }
        return dp[grid.length - 1][grid[0].length - 1];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：6ms，在所有java提交中击败了5.08%的用户。

内存消耗：42.4MB，在所有java提交中击败了34.80%的用户。

## 官方解题

&emsp;官方除了二维动态规划，还提出了一维动态规划，利用题目限定的只能右移和下移特性。

```java
class Solution {
    public int minPathSum(int[][] grid) {
        if (grid == null || grid.length == 0) return 0;

        int m = grid.length, n = grid[0].length;
        int[] dp = new int[n];
        // 初始化
        dp[0] = grid[0][0];
        for (int i = 1; i < n; i++) {
            dp[i] = dp[i - 1] + grid[0][i];
        }

        for (int i = 1; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // 没有左边元素，最小值为上面元素加当前值
                if (j == 0) dp[j] = dp[j] + grid[i][j]; 
                // 左边和上面的最小值加当前值
                // 由于遍历顺序，dp[i]必然是上一层的最小值，dp[i - 1]必然是当前层左边的最小值
                else dp[j] = Math.min(dp[j], dp[j - 1]) + grid[i][j];
            }
        }
        return dp[n - 1];
    }
}
```

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了98.24%的用户。

内存消耗：42.4MB，在所有java提交中击败了35.61%的用户。

&emsp;官方还提出了空间复杂度为$O(1)$的算法，只不过是在原数组操作，依然是二维动态规划。