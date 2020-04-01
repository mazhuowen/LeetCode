[toc]

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.



## 题目解读

&emsp;不能偷相邻的房子，否则触发警报。求出可以偷到的最大金额。

```java
class Solution {
    public int rob(int[] nums) {
        
    }
}
```

## 程序设计

* 动态规划记录当前房子可以偷的最大金额。

```java
class Solution {
    public int rob(int[] nums) {
        if (nums == null || nums.length == 0) return 0;

        int maxMon = 0;
        // 记录当前位置可以盗窃的最大金额
        int[] dp = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            // 0，1位置需要初始化
            if (i == 0 || i == 1) dp[i] = nums[i];

            // 更新当前房子的最大值
            if (i - 2 >= 0) dp[i] = dp[i - 2] + nums[i];
            if (i - 1 >= 0) dp[i] = Math.max(dp[i], dp[i - 1]);
            // 更新金额
            maxMon = Math.max(maxMon, dp[i]);
        }
        return maxMon;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.1MB，在所有java提交中击败了5.09%的用户。

## 官方解题

&emsp;官方在动态规划基础上做了进一步优化，实际上每次只需记录前两个的最大金额，不需要数组记录，优化如下：

```java
public int rob(int[] num) {
    int prevMax = 0;
    int currMax = 0;
    for (int x : num) {
        int temp = currMax;
        currMax = Math.max(prevMax + x, currMax);
        prevMax = temp;
    }
    return currMax;
}
```