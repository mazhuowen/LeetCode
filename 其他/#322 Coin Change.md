[toc]

You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

**Note**:
You may assume that you have an infinite number of each kind of coin.



## 题目解读

&emsp;找零问题，币值自定义，需要动态规划解决。

```java
class Solution {
    public int coinChange(int[] coins, int amount) {

    }
}
```

## 程序设计

* 问题粗略的看是完全背包问题，每个硬币可以无限拿取，动态规划数组`dp(i,j)`表示$0 \sim i$的硬币构成$j$的最少硬币数，则如下：

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[][] dp = new int[coins.length + 1][amount + 1];
        for (int[] array : dp) Arrays.fill(array, Integer.MAX_VALUE);
        // 初始化价值为0的硬币组合数为0
        for (int i = 0; i <= coins.length; i++) dp[i][0] = 0;

        for (int i = 1; i <= coins.length; i++) {
            for (int j = 0; j <= amount; j++) {
                dp[i][j] = dp[i - 1][j];
                for (int k = 1; k * coins[i - 1] <= j; k++) {
                    if (dp[i - 1][j - k * coins[i - 1]] == Integer.MAX_VALUE) continue;

                    dp[i][j] = Math.min(dp[i][j], dp[i - 1][j - k * coins[i - 1]] + k);
                }
            }
        }
        return dp[coins.length][amount] < Integer.MAX_VALUE ? dp[coins.length][amount] : -1;
    }
}
```

* 事实上该问题为背包问题的组合排列形式，由于本题只需求最少的组合数，故上述完全背包形式的解法能得到正确的答案；对于组合问题的变种，只需将内外层循环调换位置，动态规划数组也发生变化，`dp(i)`表示金额为$i$的最少组合数目。

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0;

        // 组合问题内外层循环互换
        for (int i = 1; i <= amount; i++) {
            for (int coin : coins) {
                if (coin > i || dp[i - coin] == Integer.MAX_VALUE) continue;
                dp[i] = Math.min(dp[i], dp[i - coin] + 1);
            }
        }

        return dp[amount] < Integer.MAX_VALUE ? dp[amount] : -1;
    }
}
```

## 性能分析

&emsp;原始背包问题时间复杂度为$O(NM^2)$，空间复杂度为$O(NM)$。

执行用时：414ms，在所有java提交中击败了5.00%的用户。

内存消耗：40MB，在所有java提交中击败了8.12%的用户。

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(N)$。其中$M$为硬币数。

执行用时：16ms，在所有java提交中击败了52.11%的用户。

内存消耗：39.4MB，在所有java提交中击败了51.74%的用户。

## 官方解题

&emsp;参考官方解题。