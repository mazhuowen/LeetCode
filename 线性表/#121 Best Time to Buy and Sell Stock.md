[toc]

Say you have an array for which the ith element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.



## 题目解读

&emsp;求一次交易的情况下，获取的最大收益。

```java
class Solution {
    public int maxProfit(int[] prices) {

    }
}
```

## 程序设计

* 双指针记录当前遍历区间内最大最小值；当遇到更大的值时更新最大值，当遇到更小的值时，由于时序性，重置最大最小值。

```java
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length <= 1) return 0;

        int profit = 0;
        int curMin = prices[0], curMax = prices[0];
        for (int k = 0; k < prices.length; k++) {
            // 更新最大值
            if (prices[k] > curMax) curMax = prices[k];
            // 发现比最小值更小的值，重新定位
            else if (prices[k] <= curMin) {
                profit = Math.max(profit, curMax - curMin);
                curMin = curMax = prices[k];
            }
        }
        // 不能忘记最后一段区间的比较
        return Math.max(profit, curMax - curMin);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了63.08%的用户。

内存消耗：39.8MB，在所有java提交中击败了5.32%的用户。

## 官方解题

&emsp;思路同上。