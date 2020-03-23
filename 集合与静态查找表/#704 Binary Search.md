[toc]

Given a **sorted** (in ascending order) integer array `nums` of `n` elements and a `target` value, write a function to search `target` in `nums`. If `target` exists, then return its index, otherwise return `-1`.

Note:

* You may assume that all elements in `nums` are unique.
* `n` will be in the range `[1, 10000]`.
* The value of each element in `nums` will be in the range `[-9999, 9999]`.



## 题目解读

&emsp;二分查找。

```java
class Solution {
    public int search(int[] nums, int target) {

    }
}
```

## 程序设计

* 二分查找。

```java
class Solution {
    public int search(int[] nums, int target) {
        if (nums == null || nums.length == 0) return -1;
        
        int left = 0, right = nums.length - 1;
        if (target > nums[right] || target < nums[left]) return -1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] == target) return mid;
            if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：42.5MB，在所有java提交中击败了5.15%的用户。

## 官方解题

&emsp;同上。