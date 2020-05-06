[toc]

Given an array of integers `nums`, write a method that returns the "pivot" index of this array.

We define the pivot index as the index where the sum of the numbers to the left of the index is equal to the sum of the numbers to the right of the index.

If no such index exists, we should return -1. If there are multiple pivot indexes, you should return the left-most pivot index.



Note:

* The length of `nums` will be in the range `[0, 10000]`.
* Each element `nums[i]` will be an integer in the range `[-1000, 1000]`.



## 题目解读

&emsp;寻找切分点，使得左右两部分元素之和相等。

```java
class Solution {
    public int pivotIndex(int[] nums) {

    }
}
```

## 程序设计

* 前缀和相加，遍历判断。

```java
class Solution {
    public int pivotIndex(int[] nums) {
        if (nums == null || nums.length == 0) return -1;
        int[] preSum = new int[nums.length + 1];
        for (int i = 0; i < nums.length; i++) {
            preSum[i + 1] = preSum[i] + nums[i];
        }
        // 遍历判断
        for (int i = 0; i < nums.length; i++) {
            if (preSum[i] == preSum[preSum.length - 1] - preSum[i + 1]) return i;
        }
        return -1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：40.4MB，在所有java提交中击败了28.57%的用户。

## 官方解题

&emsp;注意到前缀和只需要当前和最后总的和，官方巧妙利用这个特性将空间复杂度继续优化。

```java
class Solution {
    public int pivotIndex(int[] nums) {
        int sum = 0, leftsum = 0;
        for (int x: nums) sum += x;
        for (int i = 0; i < nums.length; ++i) {
            if (leftsum == sum - leftsum - nums[i]) return i;
            leftsum += nums[i];
        }
        return -1;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：40.2MB，在所有java提交中击败了35.71%的用户。