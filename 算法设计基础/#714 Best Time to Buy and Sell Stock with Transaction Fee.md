[toc]

Your are given an array of integers `prices`, for which the `i`-th element is the price of a given stock on day `i`; and a non-negative integer fee representing a transaction fee.

You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction. You may not buy more than 1 share of a stock at a time (ie. you must sell the stock share before you buy again.)

Return the maximum profit you can make.



**Note:**

* $0 < \text{prices.length} \le 50000$.
* $0 < \text{prices[i]} < 50000$.
* $0 \le fee < 50000$.



## 题目解读

&emsp;交易存在手续费，求最大的股票收益。

```java
class Solution {
    public int maxProfit(int[] prices, int fee) {

    }
}
```

## 程序设计

* 在原来的基础上减去交易费即可。

```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        if (prices == null || prices.length == 0) return 0;

        int n = prices.length;
        int[][] dp = new int[n][2];
        dp[0][0] = 0;
        dp[0][1] = -prices[0] - fee;
        for (int i = 1; i < n; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
            // 减去交易费
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] - prices[i] - fee);
        }

        return dp[n - 1][0];
    }
}
```

* 优化空间得：

```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        if (prices == null || prices.length == 0) return 0;

        int n = prices.length;
        int i0 = 0, i1 = -prices[0] - fee;
        for (int i = 1; i < n; i++) {
            int temp = i0;
            i0 = Math.max(i0, i1 + prices[i]);
            i1 = Math.max(i1, temp - prices[i] - fee);
        }

        return i0;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：24ms，在所有java提交中击败了27.89%的用户。

内存消耗：49.2MB，在所有java提交中击败了100.00%的用户。

&emsp;优化后时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：4ms，在所有java提交中击败了99.96%的用户。

内存消耗：49.1MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。