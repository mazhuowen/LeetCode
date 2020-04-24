[toc]

Say you have an array for which the `i`th element is the price of a given stock on day `i`.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times) with the following restrictions:

* You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
* After you sell your stock, you cannot buy stock on next day. (ie, cooldown 1 day)



## 题目解读

&emsp;股票交易存在冷冻期，求最大收益。

```java
class Solution {
    public int maxProfit(int[] prices) {

    }
}
```

## 程序设计

* [#123 Best Time to Buy and Sell Stock III](./#123 Best Time to Buy and Sell Stock III.md)中$k$为无限制的情况。

```java
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length <= 0) return 0;

        int n = prices.length;
        int[][] dp = new int[n][2];
        for (int i = 0; i < n; i++) {
            if (i == 0) {
                dp[i][0] = 0;
                dp[i][1] = -prices[i];
            }
            // 注意为1时的情况
            else if (i == 1) {
                dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
                dp[i][1] = Math.max(dp[i - 1][1], -prices[i]);
            } else {
                dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
                // 冷冻期，只能从前两天后买入
                dp[i][1] = Math.max(dp[i - 1][1], dp[i - 2][0] - prices[i]);
            }
        }

        return dp[n - 1][0]; 
    }
}
```

* 继续优化，由于状态只依赖前两个，可以用常量来代替。

```java
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length <= 0) return 0;

        int n = prices.length;
        int pre0 = 0, cur0 = 0, cur1 = -prices[0];
        for (int i = 1; i < n; i++) {
            int temp = cur0;
            cur0 = Math.max(cur0, cur1 + prices[i]);
            cur1 = Math.max(cur1, pre0 - prices[i]);
            pre0 = temp;
        }

        return cur0; 
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$，优化后时间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了99.14%的用户。

内存消耗：38.2MB，在所有java提交中击败了11.11%的用户。

## 官方解题

&emsp;暂无，密切关注。