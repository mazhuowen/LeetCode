[toc]

You may recall that an array `A` is a mountain array if and only if:

* A.length >= 3

* There exists some `i` with $0 < i < \text{A.length} - 1$ such that:

  * $A[0] < A[1] < \cdots < A[i-1] < A[i]$
  * $A[i] > A[i+1] > \cdots > A[A.length - 1]$

Given a mountain array `mountainArr`, return the **minimum** `index` such that `mountainArr.get(index) == target`.  If such an `index` doesn't exist, return `-1`.

**You can't access the mountain array directly**.  You may only access the array using a `MountainArray` interface:

* `MountainArray.get(k)` returns the element of the array at index `k` (0-indexed).
* `MountainArray.length()` returns the length of the array.

Submissions making more than `100` calls to `MountainArray.get` will be judged Wrong Answer.  Also, any solutions that attempt to circumvent the judge will result in disqualification.



Constraints:

* $3 \le \text{mountain_arr.length()} \le 10000$
* $0 \le \text{target} \le 10^9$
* $0 \le \text{mountain_arr.get(index)} \le 10^9$



## 题目解读

&emsp;在山峰数组中查询数值的索引，有多个数值则返回索引较小的，不存在返回0。

```java
/**
 * // This is MountainArray's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface MountainArray {
 *     public int get(int index) {}
 *     public int length() {}
 * }
 */
 
class Solution {
    public int findInMountainArray(int target, MountainArray mountainArr) {
        
    }
}
```

## 程序设计

* 首先二分查找得到山峰索引，后续问题转化为在有序数组二分查找数值的问题。

```java
class Solution {
    public int findInMountainArray(int target, MountainArray mountainArr) {
        int n = mountainArr.length();
        // 查找山峰值
        int split = findMountainIndex(mountainArr, n);
        // 正序二分查找前半部分
        int idx = binarySearch(mountainArr, 0, split, target, true);
        if (idx != -1) return idx;
        // 反序二分查找后半部分
        idx = binarySearch(mountainArr, split, n - 1, target, false);
        return idx;
    }

    private int findMountainIndex(MountainArray mountainArr, int n) {
        int left = 0, right = n - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (mountainArr.get(mid) > mountainArr.get(mid + 1)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    private int binarySearch(MountainArray mountainArr, int left, int right, int target, boolean flag) {
        while (left <= right) {
            int mid = left + (right - left) / 2;
            int num = mountainArr.get(mid);
            if (num == target) return mid;
            if (num < target || (!flag && num > target)) {
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

内存消耗：39.5MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;