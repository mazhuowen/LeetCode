[toc]

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle**. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.



## 题目解读

&emsp;与[#198 House Robber](./#198 House Robber.md)不同之处在于房屋是首尾相连的环。

```java
class Solution {
    public int rob(int[] nums) {

    }
}
```

## 程序设计

* 由于首尾连接，第一个房子盗窃则最后一个房子不能盗窃，反之亦然；可以分别除去首尾，然后进行[#198 House Robber](./#198 House Robber.md)中的动态规划，最后选择最大值。


```java
class Solution {
    public int rob(int[] nums) {
        if (nums == null || nums.length == 0) return 0;

        int n = nums.length;
        if (n == 1) return nums[0];

        int max = 0;
        // 去除尾
        int[] dp = new int[n];
        dp[1] = nums[0];
        for (int i = 1; i < n - 1; i++) {
            dp[i + 1] = Math.max(dp[i], dp[i - 1] + nums[i]);
        }
        max = dp[n - 1];

        // 去除头
        dp[1] = nums[1];
        for (int i = 2; i < n; i++) {
            dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]);
        }
        max = Math.max(max, dp[n - 1]);
        return max;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.7MB，在所有java提交中击败了9.09%的用户。

## 官方解题

&emsp;暂无，密切关注。