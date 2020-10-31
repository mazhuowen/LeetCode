[toc]

A peak element is an element that is greater than its neighbors.

Given an input array `nums`, where `nums[i] ≠ nums[i+1]`, find a peak element and return its index.

The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

You may imagine that `nums[-1] = nums[n] = -∞`.



**Note:**

Your solution should be in logarithmic complexity.



## 题目解读

&emsp;峰值元素是指其值大于左右相邻值的元素。给定数组任意相邻的数都不相同，找出任意的峰值元素。

```java
class Solution {
    public int findPeakElement(int[] nums) {
        
    }
}
```

## 程序设计

* 由于题目要求时间复杂度为$O(\log_2N)$，首先想到的是二分查找。对于不符合要求的点，收缩区间，继续二分查找，直到满足要求。

```java
class Solution {
    public int findPeakElement(int[] nums) {
        int left = 0, right = nums.length - 1;
        while (left <= right) {
            int mid = (left + right) / 2;
            // 比左边小，不符合要求，左边可能是更好的选择
            if (mid - 1 >= 0 && nums[mid] < nums[mid - 1]) {
                right = mid - 1;
                continue;
            }
            // 比右边小，不符合要求，右边可能是更好的选择
            if (mid + 1 < nums.length && nums[mid] < nums[mid + 1]) {
                left = mid + 1;
                continue;
            }
            return mid;
        }
        // 根据题目任意相邻数不相同，必定存在峰值，这儿不会走
        return -1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.6MB，在所有java提交中击败了20.96%的用户。

## 官方解题

&emsp;官方首先提供了最基础的顺序查找，找到第一个大于后继值的点（第一个意味着之前的点都小于当前点），当前点就是峰值：

```java
public class Solution {
    public int findPeakElement(int[] nums) {
        for (int i = 0; i < nums.length - 1; i++) {
            if (nums[i] > nums[i + 1])
                return i;
        }
        return nums.length - 1;
    }
}
```

该算法时间复杂度不满足要求，官方提供了二分查找的思路更具有几何意义，由于数组两端$-1$、$n$设定为最小值，而数组中相邻数字不相等，必然存在峰值。这样`left`、`right`必然包含至少一个山峰，二分查找的问题就转化为`left`、`right`从左右爬坡，最后在山顶相遇的问题：

```java
class Solution {
    public int findPeakElement(int[] nums) {
        int left = 0, right = nums.length - 1;
        while(left < right) {
            int mid = (left + right) / 2;
            // mid左边存在山峰(可能是mid，故right=mid)
            if (nums[mid] > nums[mid + 1]) {
                right = mid;
            } 
            // mid右边存在山峰(肯定不是mid，故left=mid+1)
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

内存消耗：38.6MB，在所有java提交中击败了16.91%的用户。