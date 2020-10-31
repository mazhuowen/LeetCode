[toc]

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e.,  `[0,1,2,4,5,6,7]` might become  `[4,5,6,7,0,1,2]`).

Find the minimum element.

The array may contain duplicates.



**Note**:

* This is a follow up problem to Find Minimum in Rotated Sorted Array.
* Would allow duplicates affect the run-time complexity? How and why?



## 题目解读

&emsp;[#153 Find Minimum in Rotated Sorted Array](./#153 Find Minimum in Rotated Sorted Array.md)的延续，允许重复值。

```java
class Solution {
    public int findMin(int[] nums) {
        
    }
}
```

## 程序设计

* 首先消除头尾重复值，然后就是[#153 Find Minimum in Rotated Sorted Array](./#153 Find Minimum in Rotated Sorted Array.md)中的判断逻辑。

```java
class Solution {
    public int findMin(int[] nums) {
        int left = 0, right = nums.length - 1;
        // 消除头尾重复值
        while(left < right && nums[left] == nums[right]) {
            left++;
        }
        // 数组全都是一样的值
        if(left > right) {
            return nums[right];
        }
        // 数组是增序的
        if(nums[left] <= nums[right]) {
            return nums[left];
        }
        // 记录左边界
        int index = left;
        while(left <= right) {
            int mid = (left + right) / 2;
            if(mid - 1 >= 0 && nums[mid] < nums[mid - 1]) {
                return nums[mid];
            }
            // 使用index判断还不是0
            if(nums[mid] >= nums[index]) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        // 不走这儿
        return -1;
    }
}
```

## 性能分析

&emsp;最坏情况时间复杂度为$O(N)$，平均时间复杂度为$O(\log_2N)$；空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.5MB，在所有java提交中击败了37.24%的用户。

## 官方解题

&emsp;暂无，密切关注。社区比较巧妙的方法如下：

```java
class Solution {
    public int findMin(int[] nums) {
        int left = 0, right = nums.length - 1;
        while(left <= right) {
            int mid = (left + right) / 2;
            // mid与right是顺序的，较小值在前面（需注意mid可能是第一个值，所以right = mid，不需要减一，避免走出范围）
            if (nums[mid] < nums[right]) {
                right = mid;
            } 
            // 临界点在mid和right区间
            else if (nums[mid] > nums[right]) {
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

&emsp;时间复杂度为$O(\log_2N)$，特殊情况会退化为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.1MB，在所有java提交中击败了40.49%的用户。