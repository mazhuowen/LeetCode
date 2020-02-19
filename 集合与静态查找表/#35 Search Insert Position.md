[toc]

Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.



## 题目解读

&emsp;给定有序数组，查找待插入数的位置。题目限定没有重复元素存在。

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        
    }
}
```

## 程序设计

* 最简单的是顺序查找

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        // 顺序查找，找到第一个大于等于target的值的地方，就是插入位置
        for(int i = 0; i < nums.length; i++) {
            if(target <= nums[i]) {
                return i ;
            }
        }
        // 所有数组中的元素小于target，需要插入表尾
        return nums.length;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：45MB，在所有java提交中击败了5.11%的用户。

## 官方解题

&emsp;暂无，密切关注。其实该题是[#34 Find First and Last Position of Element in Sorted Array](./#34 Find First and Last Position of Element in Sorted Array.md)的应用，根据题目描述，实际上就是寻找`target`的左边界，左边界就是插入的位置。

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        return findRange(nums, target);
    }
    // 查找target的左边界，就是要插入的位置
    private int findRange(int[] nums, int target) {
        int left = 0;
        int right = nums.length;
        while(left < right) {
            int mid = (left + right) / 2;
            // 向前迭代
            if(target <= nums[mid]) {
                right = mid;
            } 
            // 向后迭代
            else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：45.6MB，在所有java提交中击败了5.11%的用户。