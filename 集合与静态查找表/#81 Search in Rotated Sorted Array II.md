[toc]

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., `[0,0,1,2,2,5,6]` might become `[2,5,6,0,0,1,2]`).

You are given a target value to search. If found in the array return true, otherwise return false.



Follow up:

* This is a follow up problem to Search in Rotated Sorted Array, where nums may contain duplicates.
* Would this affect the run-time complexity? How and why?



## 题目解读

&emsp;[#33 Search in Rotated Sorted Array](./#33 Search in Rotated Sorted Array.md)的扩展，在数据存在重复的情况下判断是否包含`target`。

```java
class Solution {
    public boolean search(int[] nums, int target) {
        
    }
}
```

## 程序设计

* 重复数字造成的问题如`[1,3,1,1,1]`，前半段开头为1，和后半段结尾为1重复，这会导致区间判断的问题。可通过比较开头和结尾，将`left`指针指向大于结尾的数，这样就避开了重复数在开头结尾的影响，从而可以使用[#33 Search in Rotated Sorted Array](./#33 Search in Rotated Sorted Array.md)中的方法。

```java
class Solution {
    public boolean search(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        // 删去前半段序列开头和后半段序列结尾相同的数
        while(left < nums.length && nums[left] == nums[right]) {
            if(target == nums[left]) {
                return true;
            }
            left++;
        }
        while(left <= right) {
            int mid = (left + right) / 2;
            if(nums[mid] == target) {
                return true;
            }
            // mid在前半部分
            if(nums[mid] >= nums[left]) {
                if(target >= nums[left] && target <= nums[mid]) {
                    right = mid;
                } else {
                    left = mid + 1;
                }
            }
            // mid在后半部分
            else {
                if(target > nums[mid] && target <= nums[right]) {
                    left = mid + 1;
                } else {
                    right = mid;
                }
            }
        }
        return false;
    }
}
```

## 性能分析

&emsp;最糟糕的情况是数组全部是重复数字，这样遍历要花费$O(N)$的时间复杂度，否则平均时间复杂度为$O(\log_2N)$。空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.7MB，在所有java提交中击败了10.85%的用户。

## 官方解题

&emsp;暂无，密切关注。