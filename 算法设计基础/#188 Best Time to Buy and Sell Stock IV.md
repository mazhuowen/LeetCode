[toc]

Say you have an array for which the `i`-th element is the price of a given stock on day `i`.

Design an algorithm to find the maximum profit. You may complete at most **k** transactions.



Note:
You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).



## 题目解读

&emsp;求最大股票收益，最多交易$k$次。

```java
class Solution {
    public int maxProfit(int k, int[] prices) {

    }
}
```

## 程序设计

* [#123 Best Time to Buy and Sell Stock III](./#123 Best Time to Buy and Sell Stock III.md)的进阶，是其一般化形式。超出内存限制。

```java
class Solution {
    public int maxProfit(int k, int[] prices) {
        if (k < 1 || prices == null || prices.length <= 1) return 0;

        int n = prices.length;
        // 表示当前i天交易数为j是否持有股票
        int[][][] dp = new int[n][k + 1][2];

        for (int i = 0; i < n; i++) {
            for (int j = 1; j <= k; j++) {
                if (i == 0) {
                    dp[i][j][0] = 0;
                    dp[i][j][1] = -prices[i];
                } else {
                    dp[i][j][0] = Math.max(dp[i - 1][j][0], dp[i - 1][j][1] + prices[i]);
                    dp[i][j][1] = Math.max(dp[i - 1][j][1], dp[i - 1][j - 1][0] - prices[i]);
                }
            }
        }

        return dp[n - 1][k][0];
    }
}
```

* 优化后还是超出内存限制。

```java
class Solution {
    public int maxProfit(int k, int[] prices) {
        if (k < 1 || prices == null || prices.length <= 1) return 0;

        int n = prices.length;
        int[][] dp = new int[k + 1][2];

        for (int i = 0; i < n; i++) {
            for (int j = k; j >= 1; j--) {
                if (i == 0) {
                    dp[j][0] = 0;
                    dp[j][1] = -prices[i];
                } else {
                    dp[j][0] = Math.max(dp[j][0], dp[j][1] + prices[i]);
                    dp[j][1] = Math.max(dp[j][1], dp[j - 1][0] - prices[i]);
                }
            }
        }

        return dp[k][0];
    }
}
```

* 继续优化，当$k$过大时，相当于对交易次数没有限制，转化为[#122 Best Time to Buy and Sell Stock II](./#122 Best Time to Buy and Sell Stock II.md)的问题。

```java
class Solution {
    public int maxProfit(int k, int[] prices) {
        if (k < 1 || prices == null || prices.length <= 1) return 0;

        int n = prices.length;

        // 优化，k大于等于股票的一半时，相当于没有交易限制，转化为122的问题，寻找全局最大解
        if (k >= n / 2) {
            int profit = 0;
            for (int i = 0; i < n - 1; i++) {
                if (prices[i] < prices[i + 1]) profit += prices[i + 1] - prices[i];
            }
            return profit;
        }

        int[][] dp = new int[k + 1][2];

        for (int i = 0; i < n; i++) {
            for (int j = k; j >= 1; j--) {
                if (i == 0) {
                    dp[j][0] = 0;
                    dp[j][1] = -prices[i];
                } else {
                    dp[j][0] = Math.max(dp[j][0], dp[j][1] + prices[i]);
                    dp[j][1] = Math.max(dp[j][1], dp[j - 1][0] - prices[i]);
                }
            }
        }

        return dp[k][0];
    }
}
```

## 性能分析

&emsp;$k$过大时，时间复杂度为$O(N)$，空间复杂度为$O(1)$；$k$走动态规划时，时间复杂度为$O(KN)$，空间复杂度为$O(K)$。

执行用时：4ms，在所有java提交中击败了88.31%的用户。

内存消耗：39.9MB，在所有java提交中击败了11.11%的用户。

## 官方解题

&emsp;暂无，密切关注。