[toc]

There is a fence with $n$ posts, each post can be painted with one of the $k$ colors.

You have to paint all the posts such that no more than two adjacent fence posts have the same color.

Return the total number of ways you can paint the fence.



**Note**:
$n$ and $k$ are non-negative integers.



## 题目解读

&emsp;给栅栏着色，相同的颜色不能涂在超过两个连续的栅栏上，返回所有涂色方案。

```java
class Solution {
    public int numWays(int n, int k) {

    }
}
```

## 程序设计

* 由于可用暴力回溯尝试所有组合，而暴力回溯有三个状态：当前柱子、当前涂色、当前涂色的连续柱子数；可使用动态规划数组`dp(i,j,k)`来表示第$i$个柱子涂色$k$，前面相邻颜色相同的柱子数为$k$，$1 \le k \le 2$。
* 这样当前柱子涂色要么选择之前柱子涂色不同的方案，要么选择之前涂色相同但只有一个柱子的方案。

```java
class Solution {
    public int numWays(int n, int k) {
        if (n <= 0 || k <= 0) return 0;
        
        if (n == 1) return k;
        if (n == 2) return k * k;

        int[][][] dp = new int[n][k][2];
        // 初始化第一个柱子涂色为j的方案为1
        for (int[] temp : dp[0]) {
            temp[0] = 1;
            temp[1] = 0;
        }

        for (int i = 0; i < n - 1; i++) {
            for (int j = 0; j < k; j++) {
                for (int l = 0; l < k; l++) {
                    // 相同颜色不能超过两个
                    if (l == j) dp[i + 1][l][1] += dp[i][j][0];
                    // 不同颜色
                    else dp[i + 1][l][0] += dp[i][j][0] + dp[i][j][1];
                }
            }
        }

        // 统计结果
        int count = 0;
        for (int[] temp : dp[n - 1]) {
            count += temp[0];
            count += temp[1];
        }
        return count;
    }
}
```

> 上述思路当$n$远小于$k$时会超时，能通过是由于测试用例不完备。

* 上述动态规划模拟每种涂色，当$k$较大时会超时，实际上并不关注涂色方案，只关注当前柱子与之前柱子是否颜色一致，可优化动态规划数组为`dp(i,j)`，其中$j \in \{1, 2\}$表示第$i$个柱子与之前柱子颜色不同的涂色方案、相同的涂色方案。

```java
class Solution {
    public int numWays(int n, int k) {
        if (n <= 0 || k <= 0) return 0;

        int[][] dp = new int[n][2];
        // 第一个柱子涂色方案为k
        dp[0][0] = k;
        dp[0][1] = 0;

        for (int i = 1; i < n; i++) {
            // 颜色不同，则只需选择与之前柱子不同颜色的k-1种
            dp[i][0] = (dp[i - 1][0] + dp[i - 1][1]) * (k - 1);
            // 颜色相同，由于不能超过两个，选择之前柱子只有一个的数目
            dp[i][1] = dp[i - 1][0];
        }
        return dp[n - 1][0] + dp[n - 1][1];
    }
}
```

* 可对上述思路进行空间优化：

```java
class Solution {
    public int numWays(int n, int k) {
        if (n <= 0 || k <= 0) return 0;

        // 表示之前连续柱子颜色相同的数目
        int dp1 = k, dp2 = 0;

        for (int i = 1; i < n; i++) {
            int temp = dp1;
            dp1 = (dp1 + dp2) * (k - 1);
            dp2 = temp;
        }
        return dp1 + dp2;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NK^2)$，空间复杂度为$O(NK)$。

执行用时：14ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.8MB，在所有java提交中击败了100.00%的用户。

&emsp;优化后的时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.2MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。