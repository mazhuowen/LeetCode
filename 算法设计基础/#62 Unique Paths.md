[toc]

A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?



Constraints:

* $1 \le m, n \le 100$
* It's guaranteed that the answer will be less than or equal to $2 * 10 ^ 9$.



## 题目解读

&emsp;机器人只能向右向下移动，找到到达右下的所有路径数目。

```java
class Solution {
    public int uniquePaths(int m, int n) {

    }
}
```

## 程序设计

* 仔细分析该问题可以分为一些列子问题，可使用分治法，考虑到重复计算，使用动态规划最佳。动态规划数组保存到当前位置的路径和，即其上一个位置和左边位置路径和之和。
* 考虑到二维数组的遍历顺序，可使用一维数组记录路径和。

```java
class Solution {
    public int uniquePaths(int m, int n) {
        if (m <= 0 || n <= 0) return 0;
        int[] dp = new int[n];
        // 第一行初始化，路径只有一条，即起点一直向右
        for (int i = 0; i < n; i++) {
            dp[i] = 1;
        }
        for (int i = 1; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // 如果是第一列则路径等于上面格子的路径，不变化
                // 否则路径为右边和上边的和
                if (j != 0) dp[j] += dp[j - 1];
            }
        }
        return dp[n - 1];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.6MB, 在所有java提交中击败了5.12%的用户。

## 官方解题

&emsp;暂无，密切关注。