[toc]

Given an integer array sorted in ascending order, write a function to search `target` in `nums`.  If `target` exists, then return its index, otherwise return `-1`. **However, the array size is unknown to you**. You may only access the array using an `ArrayReader` interface, where `ArrayReader.get(k)` returns the element of the array at index k (0-indexed).

You may assume all integers in the array are less than `10000`, and if you access the array out of bounds, `ArrayReader.get` will return `2147483647`.



Note:

* You may assume that all elements in the array are unique.
* The value of each element in the array will be in the range `[-9999, 9999]`.



## 题目解读

&emsp;二分查找的接口版。

```java
/**
 * // This is ArrayReader's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface ArrayReader {
 *     public int get(int index) {}
 * }
 */

class Solution {
    public int search(ArrayReader reader, int target) {
        
    }
}
```

## 程序设计

* 简单二分查找

```java
class Solution {
    public int search(ArrayReader reader, int target) {
        // 不符合范围
        if(target >= 10_000 || target <= -10_000) return -1;
        int left = 0, right = 19_999;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if(reader.get(mid) == target) return mid;
            if(reader.get(mid) > target) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return -1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了99.40%的用户。

内存消耗：42.2MB，在所有java提交中击败了7.14%的用户。

## 官方解题

&emsp;基本同上。