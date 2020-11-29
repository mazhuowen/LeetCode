[toc]

Say you have an array for which the `i`th element is the price of a given stock on day `i`.

Design an algorithm to find the maximum profit. You may complete at most **two** transactions.



**Note**: You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).



## 题目解读

&emsp;两次交易的最大收益。

```java
class Solution {
    public int maxProfit(int[] prices) {

    }
}
```

## 程序设计

* 三维动态规划数组表示天数、完成交易数、是否持有股票三个状态。

```java
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length <= 1) return 0;

        int n = prices.length;
        // 表示截止每一天的交易次数和交易状态
        // i表示当前天，j表示截止当前完成的股票交易次数（买入股票次数），k表示是否持有股票
        int[][][] dp = new int[n][3][2];

        for (int i = 0; i < n; i++) {
            // j为0表示完成数为0，其最大收益也为0，从1开始更新
            for (int j = 1; j <= 2; j++) {
                // 初始化
                if (i == 0) {
                    dp[i][j][0] = 0;
                    dp[i][j][1] = -prices[i];
                } else {
                    // 当前天不持有股票为前一天不持有股票及前一天持有股票并在当前天卖出的最大值
                    dp[i][j][0] = Math.max(dp[i - 1][j][0], dp[i - 1][j][1] + prices[i]);
                    // 当前天持有股票为前一天持有股票及前一天不持有股票并在当前天买入的最大值
                    dp[i][j][1] = Math.max(dp[i - 1][j][1], dp[i - 1][j - 1][0] - prices[i]);
                }
            }
        }
        return dp[n - 1][2][0];
    }
}
```

* 由上述程序可知，每次都和前一状态有关，不必把保存所有的天数，简化为：

```java
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length <= 1) return 0;

        int n = prices.length;
        // 表示每一天剩余的交易次数和交易状态
        // i表示截止当前完成的股票交易次数，j表示是否持有股票
        int[][] dp = new int[3][2];

        for (int i = 0; i < n; i++) {
            // j为0表示完成数为0，其最大收益也为0，从1开始更新
            for (int j = 2; j >=1; j--) {
                // 初始化
                if (i == 0) {
                    dp[j][0] = 0;
                    dp[j][1] = -prices[i];
                } else {
                    // 当前天不持有股票为前一天不持有股票及前一天持有股票并在当前天卖出的最大值
                    dp[j][0] = Math.max(dp[j][0], dp[j][1] + prices[i]);
                    // 当前天持有股票为前一天持有股票及前一天不持有股票并在当前天买入的最大值
                    dp[j][1] = Math.max(dp[j][1], dp[j - 1][0] - prices[i]);
                }
            }
        }
        return dp[2][0];
    }
}
```

* 上述数组可以继续简化为常量，分别表示之前完成i笔，当前是否持有股票。

```java
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length <= 1) return 0;

        int n = prices.length;
        // i0j0始终是0，i2j1不需要
        int i0j1 = -prices[0], i1j0 = 0, i1j1 = -prices[0], i2j0 = 0;

        for (int i = 1; i < n; i++) {
            i2j0 = Math.max(i2j0, i1j1 + prices[i]);
            
            i1j1 = Math.max(i1j1, i1j0 - prices[i]);
            i1j0 = Math.max(i1j0, i0j1 + prices[i]);
            // i0j0始终为0
            i0j1 = Math.max(i0j1, 0 - prices[i]);
        }
        return i2j0;
    }
}
```

## 性能分析

&emsp;原始动态规划数组时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：6ms，在所有java提交中击败了53.71%的用户。

内存消耗：41.2MB，在所有java提交中击败了28.57%的用户。

&emsp;优化后时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：3ms，在所有java提交中击败了88.91%的用户。

内存消耗：39.7MB，在所有java提交中击败了57.14%的用户。

## 官方解题

&emsp;暂无，密切关注。