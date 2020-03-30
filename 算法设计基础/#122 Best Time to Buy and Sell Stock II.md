[toc]

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

Note: You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).



## 题目解读

&emsp;给定时间和股票价格表，计算买股票的最大收益。

```java
class Solution {
    public int maxProfit(int[] prices) {

    }
}
```

## 程序设计

* 股票的最大收益可以分解为局部问题，即如果当天比之后价格低就买入，全局解就是局部解之和。

```java
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length <= 1) return 0;

        int profit = 0;
        for (int i = 0; i < prices.length - 1; i++) {
            // 局部最佳
            if (prices[i] < prices[i + 1]) profit += prices[i + 1] - prices[i];
        }
        return profit;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了99.21%的用户。

内存消耗：39.9MB，在所有java提交中击败了5.02%的用户。

## 官方解题

&emsp;同上，还提供了其它思路。