[toc]

Given an integer array with all positive numbers and no duplicates, find the number of possible combinations that add up to a positive integer target.



**Follow up**:

* What if negative numbers are allowed in the given array?
* How does it change the problem?
* What limitation we need to add to the question to allow negative numbers?



## 题目解读

&emsp;给定数组，查询所有组合使得和为目标值。

```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        
    }
}
```

## 程序设计

* 问题为背包问题变形的排列组合问题，需要调换内外循环。

```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        int[] dp = new int[target + 1];
        dp[0] = 1;

        for (int i = 1; i <= target; i++) {
            for (int num : nums) {
                // 与完全背包问题区别，不再遍历k
                if (num <= i) dp[i] += dp[i - num];
            }
        }

        return dp[target] == Integer.MAX_VALUE ? -1 : dp[target];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(M)$。

执行用时：1ms，在所有java提交中击败了99.79%的用户。

内存消耗：36.7MB，在所有java提交中击败了97.79%的用户。

## 官方解题

&emsp;暂无，密切关注。