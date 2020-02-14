[toc]

Given an array of integers `nums` sorted in ascending order, find the starting and ending position of a given `target` value.

Your algorithm's runtime complexity must be in the order of $O(\log_2n)$.

If the target is not found in the array, return `[-1, -1]`.



## 题目解读

&emsp;给定已排序的数组，数值有重复，在排序数组中查找元素的第一个和最后一个位置。没有则返回-1。

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        
    }
}
```

## 程序设计

* 首先根据二分查找，找到一个值，然后向左右遍历记录相同值的索引。

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int start = 0;
        int end = nums.length - 1;
        int[] index = new int[]{-1, -1};
        while(start <= end) {
            int mid = (start + end) / 2;
            // 找到，则向左右遍历相同的值
            if(nums[mid] == target) {
                int i = mid;
                // 向左遍历
                while((i - 1) >= 0 && nums[i - 1] == nums[mid]) {
                    i--;
                }
                index[0] = i;
                i = mid;
                // 向右遍历
                while((i + 1) < nums.length && nums[i + 1] == nums[mid]) {
                    i++;
                }
                index[1] = i;
                return index;
            }
            if(target > nums[mid]) {
                start = mid + 1;
            } else {
                end = mid - 1;
            }
        }
        return index;
    }
}
```

## 性能分析

&emsp;时间复杂度平均为$O(\log_2N)$，最坏为$O(N)$，空间复杂度为$O(1)$。实际上不满足题目条件。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：48.2MB，在所有java提交中击败了5.01%的用户。

## 官方解题

&emsp;官方思路是使用二分查找查找边界。

