[toc]

Given an integer array `arr`, remove a subarray (can be empty) from `arr` such that the remaining elements in `arr` are **non-decreasing**.

A subarray is a contiguous subsequence of the array.

Return the length of the shortest subarray to remove.



**Constraints:**

- $1 \le \text{arr.length} \le 10^5$
- $0 \le \text{arr[i]} \le 10^9$



## 题目解读

&emsp;给定数组，删掉一段子数组使得剩余的数组数字非递减。求需要删除数组的最小长度。

```java
class Solution {
    public int findLengthOfShortestSubarray(int[] arr) {

    }
}
```

## 程序设计

* 由于只需要删除一段子数组，使得剩余的数组数字非递减，则情况如下：
  * 首部非递减，删除剩余数组；
  * 尾部非递减，删除剩余数组；
  * 首尾组成非递减数组，删除中间数组。
* 对于首端、尾端的非递减数组，只需遍历判断即可；对于中间数组的删除，由于拼接位置，左端数组的值必然小于等于右端数组的值，可使用双指针遍历更新，如果满足条件则更新删除长度，不满足条件则右指针右移。

```java
class Solution {
    public int findLengthOfShortestSubarray(int[] arr) {
        int n = arr.length;
        int left = 0, right = n - 1;
        // 检查左侧非递减数组
        while (left < n - 1 && arr[left] <= arr[left + 1]) left++;
        // 整体是有序的，返回0
        if (left == n - 1) return 0; 

        // 检查右侧非递减数组
        while (right >= 1 && arr[right - 1] <= arr[right]) right--;
        // 保留左侧或保留右侧的最少代价
        int res = Math.min(n - left - 1, right);

        // 查找删除中间区间的情况
        int l = 0, r = right;
        while (l <= left && r <= n - 1) {
            // 左侧小于等于右侧，可构成非递减数组，左侧迭代
            if (arr[l] <= arr[r]) {
                // 删除l~r之间的数组
                res = Math.min(res, r - l - 1);
                l++;
            } 
            // 左侧大于右侧，右侧迭代
            else {
                r++;
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：57.6MB，在所有java提交中击败了68.65%的用户。

## 官方解题

&emsp;暂无，密切关注。