[toc]

Given an array containing $n$ distinct numbers taken from $0, 1, 2, \dots, n$, find the one that is missing from the array.



Note:
Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?



## 题目解读

&emsp;给定数组包含$n$个数，是$0 \sim n$共$n + 1$个数中的$n$个，求缺失的那一个。

```java
class Solution {
    public int missingNumber(int[] nums) {
        
    }
}
```

## 程序设计

* 联想到[#136 Single Number](../散列表/#136 Single Number.md)中使用异或查询不重复的值，该问题可转化为这个问题，即$n$个数字再异或$0 \sim n$共$n + 1$个数，其中缺失的数是单个的，其他数字都是成对的，异或的值就是缺失值。

```java
class Solution {
    public int missingNumber(int[] nums) {
        if (nums == null || nums.length == 0) return 0;

        int res = nums.length;
        for (int i = 0; i < nums.length; i++) {
            res ^= i;
            res ^= nums[i];
        } 

        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：40.2MB，在所有java提交中击败了6.67%的用户。

## 官方解题

&emsp;官方还提供了数学方式，即利用高斯求和公式减去数组中的和。

```java
class Solution {
    public int missingNumber(int[] nums) {
        if (nums == null || nums.length == 0) return 0;

        int res = (nums.length + 1) * nums.length / 2;
        for (int i = 0; i < nums.length; i++) {
            res -= nums[i];
        } 

        return res;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：40MB，在所有java提交中击败了6.67%的用户。