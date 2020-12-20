[toc]

Given an unsorted array of integers `nums`, return the length of the longest **continuous increasing subsequence** (i.e. subarray). The subsequence must be **strictly** increasing.

A **continuous increasing subsequence** is defined by two indices $l$ and $r$ ($l < r$) such that it is $\text{[nums[l]}, \text{nums[l + 1]}, \dots, \text{nums[r - 1]}, \text{nums[r]]}$ and for each $l \le i < r$, $\text{nums[i]} < \text{nums[i + 1]}$.

 

**Example 1**:

```
Input: nums = [1,3,5,4,7]
Output: 3
Explanation: The longest continuous increasing subsequence is [1,3,5] with length 3.
Even though [1,3,5,7] is an increasing subsequence, it is not continuous as elements 5 and 7 are separated by element
4.
```

**Example 2**:

```
Input: nums = [2,2,2,2,2]
Output: 1
Explanation: The longest continuous increasing subsequence is [2] with length 1. Note that it must be strictly
increasing.
```



**Constraints**:

* $0 \le \text{nums.length} \le 10^4$
* $-10^9 \le \text{nums[i]} \le 10^9$



## 题目解读

&emsp;查找最长递增子数组的长度。

```java
class Solution {
    public int findLengthOfLCIS(int[] nums) {

    }
}
```

## 程序设计

* 双指针滑动。

```java
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        if (nums == null || nums.length == 0) return 0;

        int res = 0;
        int left = 0, right = 0;
        while (right < nums.length) {
            if (right != 0 && nums[right] <= nums[right - 1]) {
                res = Math.max(res, right - left);
                left = right;
            }
            right++;
        }
        res = Math.max(res, right - left);
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1 ms, 在所有 Java 提交中击败了99.74%的用户。

内存消耗：39.2 MB, 在所有 Java 提交中击败了69.63%的用户.

## 官方解题

&emsp;官方思路类似。
