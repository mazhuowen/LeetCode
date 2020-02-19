[toc]

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`).

You are given a target value to search. If found in the array return its index, otherwise return `-1`.

You may assume no duplicate exists in the array.

Your algorithm's runtime complexity must be in the order of O(log n).



## 题目解读

&emsp;给定数组为已排序数组截断拼接的数组，需要在$O(\log_2N)$的时间复杂度内查找数据。

```java
class Solution {
    public int search(int[] nums, int target) {
        
    }
}
```

## 程序设计

* 考虑到需要对数级别的时间复杂度，需要使用二分查找。由于数组有两部分组成，每一部分都是有序的，可以先查到分界点，然后根据目标值判断是在前面部分还是后面部分查找。

```java
class Solution {
    public int search(int[] nums, int target) {
        if(nums == null || nums.length == 0) {
            return -1;
        }
        if(nums.length == 1) {
            return nums[0] == target ? 0 : -1;
        }

        int boundary = findIndex(nums);
        // 有序，直接二分查找
        if(boundary == -1) {
            return binarySearch(nums, target, 0, nums.length - 1);
        }
        // 截断，判断
        else {
            // 在前半部分查找
            if(target >= nums[0]) {
                return binarySearch(nums, target, 0, boundary);
            } 
            // 在后半部分查找
            else {
                return binarySearch(nums, target, boundary + 1, nums.length - 1);
            }
        }
    }

    // 返回截断点索引
    private int findIndex(int[] nums) {
        int start = 0;
        int end  = nums.length - 1;
        // 有序数组，未发生截断拼接
        if(nums[start] < nums[end]) {
            return -1;
        }
        // 二分查找边界
        while(start <= end) {
            int boundary = (start + end) / 2;
            // 找到划分点
            if(nums[boundary] > nums[boundary + 1]) {
                return boundary;
            }
            // 在前半段，向后迭代
            if(nums[boundary] >= nums[start]) {
                start = boundary + 1;
            } 
            // 在后半段，向前迭代
            else {
                end = boundary - 1;
            }
        }
        // 不会走到这儿
        return 0;
    }

    private int binarySearch(int[] nums, int target, int start, int end) {
        // 未找到
        if(start > end) {
            return -1;
        }
        int mid = (start + end) / 2;
        if(nums[mid] == target) {
            return mid;
        }
        if(target > nums[mid]) {
            return binarySearch(nums, target, mid + 1, end);
        } else {
            return binarySearch(nums, target, start, mid - 1);
        }
    }
}
```

> 题目没有说明，但经测试输入存在单纯的有序数组情况，需要额外判断。

## 性能分析

&emsp;二分查找时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$，由于是尾递归。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：43.5MB，在所有java提交中击败了5.02%的用户。

## 官方解题

&emsp;官方思路一致。社区存在一次遍历完成的算法，通过一次二分找到值，同时结合了空数组、有序数组、截断数组等情况，更强大更明了。

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        while(left <= right) {
            int mid =  (left + right) / 2;
            // 找到返回
            if(nums[mid] == target){
                return mid;
            }

            // 左边升序
            if(nums[left] <= nums[mid]) {
                // 如果target正好在这个区间则查找
                if(target >= nums[left] && target <= nums[mid]) {
                    right = mid;
                }
                // 不在这个区间，向右查找
                else {
                    left = mid + 1;
                }

            }
            // 右边升序
            else {
                 // 如果target正好在这个区间则查找
                if(target > nums[mid] && target <= nums[right]) {
                    left = mid  + 1;
                }
                // 不在区间，向右查找
                else{
                    right = mid;
                }
            }
        }
        // 空数组、未找到
        return -1; 
    }
}
```