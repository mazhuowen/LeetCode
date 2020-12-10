[toc]

Your are given an array of positive integers `nums`.

Count and print the number of (contiguous) subarrays where the product of all the elements in the subarray is less than $k$.



**Example 1**:

```
Input: nums = [10, 5, 2, 6], k = 100
Output: 8
Explanation: The 8 subarrays that have product less than 100 are: [10], [5], [2], [6], [10, 5], [5, 2], [2, 6], [5, 2, 6].
Note that [10, 5, 2] is not included as the product of 100 is not strictly less than k.
```


**Note**:

* $0 < \text{nums.length} \le 50000$.
* $0 < \text{nums[i]} < 1000$.
* $0 \le k < 10^6$.



## 题目解读

&emsp;求子数组乘积小于给定值的子数组数目。

```java
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {

    }
}
```

## 程序设计

* 采用滑动窗口计算。

```java
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        if (k <= 0) return 0;

        int res = 0;
        // 滑动窗口
        int left = 0, right = 0, product = 1;
        while (right < nums.length) {
            product *= nums[right];
            // 乘积超出k，则出队
            while (left <= right && product >= k) {
                // 以left起始的合法子数组数目
                res += right - left;
                product /= nums[left++];
            }
            right++;
        }

        // 计算最后符合条件的数目，注意防止数据溢出
        res += (int)(((long)right - left + 1) * (right - left) / 2);
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：9 ms, 在所有 Java 提交中击败了97.05%的用户

内存消耗：48.2 MB, 在所有 Java 提交中击败了51.27%的用户。

## 官方解题

&emsp;官方除了上述思路还提供了二分查找的解题。
