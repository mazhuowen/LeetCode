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

&emsp;官方思路是使用二分查找查找边界。非常巧妙的改造二分查找，大体思路如下：

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int[] res = {-1, -1};
        int left = searchLeft(nums, target);
        // nums[left] != target对应没找到，left == nums.length对应空数组的情况
        if(left == nums.length || nums[left] != target) {
            return res;
        }
        // 存在target值，必然存在右边界，故无需判断
        res[0] = left;
        res[1] = searchRight(nums, target) - 1;
        return res;
    }

    // 查找左边界
    private int searchLeft(int[] nums, int target) {
        int low = 0;
        int high = nums.length - 1;
        // 在low=high的时候退出，由于查找过程high会保留target值，最后low和high回合的值要么是target值左边界，要么是没找到其他值
        while(low < high) {
            int mid = (low + high) / 2;

            // 向左
            if(target < nums[mid]) {
                high = mid - 1;
            } 
            // 由于是遍历左边界，需要保留至少一个target，最后low和high都会聚集到最左边target
            else if(target == nums[mid]) {
                high = mid;
            } 
            // 向右
            else {
                low = mid + 1;
            }
        } 
        return low;
    }

     // 查找右边界（searchRight记录左边界的后一位）
    private int searchRight(int[] nums, int target) {
        int low = 0;
        // 不是nums.length-1，因为对于连续数直到结尾的边界后一位是nums.length
        int high = nums.length;
        // low和high回合的位置要么是target右边界后一位（包括为nums.length的特殊情况），要么其他值
        while(low < high) {
            int mid = (low + high) / 2;
            // 由于是边界后一位，mid-1可能就是边界，不能包含，故high=mid
            if(target < nums[mid]) {
                high = mid;
            } 
            // mid与目标值一致，low更新为后一位（因为检索边界后一位，不包括mid）
            else if(target == nums[mid]) {
                low = mid + 1;
            } else {
                low = mid + 1;
            }
        }
        return low;
    }
}
```

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：48.3MB，在所有java提交中击败了5.08%的用户。

&emsp;官方形式在上述思路基础上对形式做了简化，由于`searchLeft`与右边界`right`无关，初始长度可设为` nums.length`，第一个迭代条件也可以改为`high = mid`，对结果无影响。这样改动后和`searchRight`形式相似，可以合并为一个方法：

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int[] res = {-1, -1};
        int left = searchRange(nums, target, true);
        // nums[left] != target对应没找到，left == nums.length对应空数组的情况
        if(left == nums.length || nums[left] != target) {
            return res;
        }
        // 存在target值，必然存在右边界，故无需判断
        res[0] = left;
        res[1] = searchRange(nums, target, false) - 1;
        return res;
    }

    // flag表示查找左边界还是右边界
    private int searchRange(int[] nums, int target, boolean flag) {
        int low = 0;
        int high = nums.length;

        while(low < high) {
            int mid = (low + high) / 2;

            if(target < nums[mid] || (flag && target == nums[mid])) {
                high = mid;
            } else {
                low = mid + 1;
            }
        } 
        return low;
    }
}
```