[toc]

In a country popular for train travel, you have planned some train travelling one year in advance.  The days of the year that you will travel is given as an array `days`.  Each day is an integer from $1$ to $365$.

Train tickets are sold in 3 different ways:

* a 1-day pass is sold for `costs[0]` dollars;
* a 7-day pass is sold for `costs[1]` dollars;
* a 30-day pass is sold for `costs[2]` dollars.

The passes allow that many days of consecutive travel.  For example, if we get a 7-day pass on day 2, then we can travel for 7 days: day 2, 3, 4, 5, 6, 7, and 8.

Return the minimum number of dollars you need to travel every day in the given list of `days`.



**Note**:

* $1 \le \text{days.length} \le 365$
* $1 \le \text{days[i]} \le 365$
* `days` is in strictly increasing order.
* $\text{costs.length} == 3$
* $1 \le \text{costs[i]} \le 1000$



## 题目解读

&emsp;给定每年出行的天数和三种通行证，求最少的花费。

```java
class Solution {
    public int mincostTickets(int[] days, int[] costs) {

    }
}
```

## 程序设计

* 动态规划数组`dp(i)`表示截止索引$i$的最少花费。

```java
class Solution {
    public int mincostTickets(int[] days, int[] costs) {
        int n = days.length;
        int[] dp = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            // 每一天都买一张通行证
            dp[i] = dp[i - 1] + costs[0];
            // 七天内买一张通行证
            int tmp = i;
            while (tmp > 0 && days[i - 1] - days[tmp - 1] < 7) tmp--;
            dp[i] = Math.min(dp[i], dp[tmp] + costs[1]);
            // 三十天内买一张通行证
            while (tmp > 0 && days[i - 1] - days[tmp - 1] < 30) tmp--;
            dp[i] = Math.min(dp[i], dp[tmp] + costs[2]);
        }
        return dp[n];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(7N)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了38.55%的用户。

内存消耗：36.3MB，在所有java提交中击败了98.01%的用户。

## 官方解题

&emsp;官方思路类似。