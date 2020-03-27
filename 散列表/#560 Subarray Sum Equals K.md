[toc]

Given an array of integers and an integer **k**, you need to find the total number of continuous subarrays whose sum equals to **k**.

Note:

* The length of the array is in range `[1, 20,000]`.
* The range of numbers in the array is `[-1000, 1000]` and the range of the integer **k** is `[-1e7, 1e7]`.



## 题目解读

&emsp;给定数组，统计数组中连续子序列之和等于$k$的个数。

```java
class Solution {
    public int subarraySum(int[] nums, int k) {

    }
}
```

## 程序设计

* 最初的想法是前缀和遍历两次计算，但是由于数组规模，必然超时，只能选取额外的空间存储信息，避免二次遍历；可以选择哈希表记录遍历过的前缀和并计数，每次遍历到一个前缀和，则判断与其差为$k$的值是否存在，存在几个，并计数更新。

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        if (nums == null || nums.length == 0) return 0;
        int count = 0;
        // 记录前缀和和次数
        Map<Integer, Integer> record = new HashMap<>();
        // 一定不能忘记前缀和初始位置0
        record.put(0, 1);
        int[] preSum = new int[nums.length + 1];
        for (int i = 0; i < nums.length; i++) {
            preSum[i + 1] = preSum[i] + nums[i];
            // 其前存在差值为K的前缀和
            if (record.get(preSum[i + 1] - k) != null) count += record.get(preSum[i + 1] - k);
            // 更新当前计数
            record.put(preSum[i + 1], record.getOrDefault(preSum[i + 1], 0) + 1);
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：22ms，在所有java提交中击败了78.52%的用户。

内存消耗：42.5MB，在所有java提交中击败了5.13%的用户。

## 官方解题

&emsp;同上。