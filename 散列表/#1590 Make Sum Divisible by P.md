[toc]

Given an array of positive integers `nums`, remove the **smallest** subarray (possibly **empty**) such that the **sum** of the remaining elements is divisible by `p`. It is **not** allowed to remove the whole array.

Return the length of the smallest subarray that you need to remove, or $-1$ if it's impossible.

A **subarray** is defined as a contiguous block of elements in the array.



**Constraints:**

- $1 \le \text{nums.length} \le 10^5$
- $1 \le \text{nums[i]} \le 10^9$
- $1 \le p \le 10^9$



## 题目解读

&emsp;求所需移除的最短子数组，使得剩余的数组和可被`p`整除。

```java
class Solution {
    public int minSubarray(int[] nums, int p) {

    }
}
```

## 程序设计

* 假设数组和为`s`，初步的思路是对于删除的某段，其和必然是`s%p+xp`，这样可利用哈希表来存储之前的前缀和，但是需要判断迭代是否存在不同的`x`使得上式成立，会超时。

```java
class Solution {
    public int minSubarray(int[] nums, int p) {
        int n = nums.length;
        // 前缀和
        long[] preSum = new long[n + 1];
        for (int i = 1; i <= n; i++) preSum[i] = preSum[i - 1] + nums[i - 1];
        // 不能被p整除的余数
        long diff = preSum[n] % p;
        if (diff == 0) return 0;

        int min = n;
        Map<Long, Integer> record = new HashMap<>();
        for (int i = 0; i <= n; i++) {
            // 迭代判断是否存在
            long tmp = diff;
            while (preSum[i] - tmp >= 0 && !record.containsKey(preSum[i] - tmp)) {
                tmp += p;
            }
            // 存在则更新
            if (preSum[i] - tmp >= 0 && record.containsKey(preSum[i] - tmp)) min = Math.min(min, i - record.get(preSum[i] - tmp));
            record.put(preSum[i], i);
        }
        return min == n ? -1 : min;
    }
}
```

* 改变思路，哈希表存储前缀和取余后的结果，这样就不需要迭代判断`x`。

```java
class Solution {
    public int minSubarray(int[] nums, int p) {
        int n = nums.length;
        // 前缀和
        long[] preSum = new long[n + 1];
        for (int i = 1; i <= n; i++) preSum[i] = preSum[i - 1] + nums[i - 1];
        // 不能被p整除的余数
        long diff = preSum[n] % p;
        if (diff == 0) return 0;

        int min = n;
        Map<Long, Integer> record = new HashMap<>();
        for (int i = 0; i <= n; i++) {
            // (preSum[i] % p + p - diff) % p的简化
            long key = (preSum[i] - diff) % p;
            // 更新
            if (record.containsKey(key)) {
                min = Math.min(min, i - record.get(key));
            }
            record.put(preSum[i] % p, i);
        }
        return min == n ? -1 : min;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：40ms，在所有java提交中击败了58.71%的用户。

内存消耗：58.6MB，在所有java提交中击败了51.24%的用户。

## 官方解题

&emsp;暂无，密切关注。