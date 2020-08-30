[toc]

Given an array `nums` and a target value $k$, find the maximum length of a subarray that sums to $k$. If there isn't one, return $0$ instead.



**Note**:
The sum of the entire nums array is guaranteed to fit within the 32-bit signed integer range.



**Follow Up:**
Can you do it in $O(n)$ time?



## 题目解读

&emsp;给定数组，找到和为$k$的最长子数组。

```java
class Solution {
    public int maxSubArrayLen(int[] nums, int k) {
        
    }
}
```

## 程序设计

* 空间换时间，使用字典记录第一次出现的前缀和，遍历比较并记录最长长度。

```java
class Solution {
    public int maxSubArrayLen(int[] nums, int k) {
        if (nums == null || nums.length == 0) return 0;

        // 记录最前面的前缀和
        Map<Integer, Integer> preSum = new HashMap<>();
        preSum.put(0, -1);
        int sum = 0, maxLen = 0;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            if (preSum.get(sum - k) != null) maxLen = Math.max(maxLen, i - preSum.get(sum - k));
            preSum.putIfAbsent(sum, i);
        }

        return maxLen;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：14ms，在所有java提交中击败了80.12%的用户。

内存消耗：41.1MB，在所有java提交中击败了30.77%的用户。

## 官方解题

&emsp;暂无，密切关注。