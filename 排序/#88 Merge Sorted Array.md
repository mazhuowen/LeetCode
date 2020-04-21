[toc]

Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.

Note:

* The number of elements initialized in nums1 and nums2 are m and n respectively.
* You may assume that nums1 has enough space (size that is greater or equal to m + n) to hold additional elements from nums2.



## 题目解读

&emsp;合并两个有序数组。

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {

    }
}
```

## 程序设计

* 归并排序中的合并操作。

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int[] temp = new int[m + n];
        int idx = 0, idx1 = 0, idx2 = 0;
        while (idx1 < m && idx2 < n) {
            if (nums1[idx1] <= nums2[idx2]) temp[idx++] = nums1[idx1++];
            else  temp[idx++] = nums2[idx2++];
        }

        while (idx1 < m) {
            temp[idx++] = nums1[idx1++];
        }
        while (idx2 < n) {
            temp[idx++] = nums2[idx2++];
        }

        for (int i = 0; i < m + n; i++) {
            nums1[i] = temp[i];
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N + M)$，空间复杂度为$O(N + M)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.4MB，在所有java提交中击败了5.06%的用户。

## 官方解题

&emsp;官方解题巧妙的从后往前遍历，这样不会引入额外空间。

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int idx = m + n - 1, idx1 = m - 1, idx2 = n - 1;
        while (idx1 >= 0 && idx2 >= 0) {
            if (nums1[idx1] > nums2[idx2]) nums1[idx--] = nums1[idx1--];
            else  nums1[idx--] = nums2[idx2--];
        }

        while (idx1 >= 0) {
            nums1[idx--] = nums1[idx1--];
        }
        while (idx2 >= 0) {
            nums1[idx--] = nums2[idx2--];
        }
    }
}
```

&emsp;时间复杂度为$O(N + M)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.4MB，在所有java提交中击败了5.06%的用户。