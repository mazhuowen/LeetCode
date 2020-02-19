[toc]

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e.,  `[0,1,2,4,5,6,7]` might become  `[4,5,6,7,0,1,2]`).

Find the minimum element.

You may assume no duplicate exists in the array.



## 题目解读

&emsp;在两截有序数组拼接的数组中找到最小数。题目限定没有重复数。

```java
class Solution {
    public int findMin(int[] nums) {
        
    }
}
```

## 程序设计

* 首先要考虑单纯增序的情况，直接返回第一个数；对于分界点使用二分查找判断。

```java
class Solution {
    public int findMin(int[] nums) {
        int left = 0, right = nums.length - 1;
        // 如果是增序，直接返回第一个元素
        if(nums[right] >= nums[left]) {
            return nums[left];
        }
        // 二分查找
        while(left <= right) {
            int mid = (left + right) / 2;
            // 找到最小数
            if(mid - 1 >= 0 && nums[mid] < nums[mid - 1]) {
                return nums[mid];
            }
            // mid在前半截
            if(nums[mid] >= nums[0]) {
                left = mid + 1;
            }
            // mid在后半截
            else {
                right = mid - 1;
            }
        }
        // 不会走这儿
        return -1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.3MB，在所有java提交中击败了17.90%的用户。

## 官方解题

&emsp;暂无，密切关注。从[#154 Find Minimum in Rotated Sorted Array II](./#154 Find Minimum in Rotated Sorted Array II.md)再回首，社区比较巧妙的解决方法如下：

```java
class Solution {
    public int findMin(int[] nums) {
        int left = 0, right = nums.length - 1;
        while(left <= right) {
            int mid = (left + right) / 2;
            // mid与right是顺序的，较小值在前面（需注意mid可能是第一个值，所以right = mid，不需要减一，避免走出范围）
            if(nums[mid] < nums[right]) {
                right = mid;
            } 
            // 临界点在mid和right区间
            else if(nums[mid] > nums[right]) {
                left = mid + 1;
            } 
            // mid所在值等于right所在值
            // 可能mid与right重合，此时left也重合，是最小值，high--跳出循环
            // 也有可能mid与right指向头尾的重复值，此时临界点在中间，right--向前移动
            else {
                right--;
            }
        }
        // left就是最小值（但不一定是临界值）
        return nums[left];
    }
}
```

