[toc]

Given an array `A` of positive integers (not necessarily distinct), return the lexicographically largest permutation that is smaller than `A`, that can be **made with one swap** (A swap exchanges the positions of two numbers `A[i]` and `A[j]`).  If it cannot be done, then return the same array.



**Note:**

* $1 \le \text{A.length} \le 10000$
* $1 \le \text{A[i]} \le 10000$



## 题目解读

&emsp;给定数组，只交换一次，使得得到的数组是比原数组字典序小的最大值，不存在则返回原数组。

```java
class Solution {
    public int[] prevPermOpt1(int[] A) {
        
    }
}
```

## 程序设计

* 首先当数组中数字递增时，交换无法得到小于原数组的值，故返回数组；其次交换数字在低位时得到的数字是小于原数组的最大数字。
* 故从数组有段开始遍历查找第一个不符合递增条件的位置，找到该位置就需要交换，采用二分查找查找小于带交换位置的最大数值并交换。

```java
class Solution {
    public int[] prevPermOpt1(int[] A) {
        if (A == null || A.length == 0) return A;
        int i = A.length - 1;
        for (; i >= 1; i--) {
            if (A[i] < A[i - 1]) break;
        }
        // 无法交换得到较小数组
        if (i == 0) return A;
		
        // 二分查找交换位置
        int j = binarySearch(A, i, A.length, A[i - 1]) - 1;
        // 关键，当存在多个一样的值时，交换最前面的数
        while (j >= 1 && A[j - 1] == A[j]) {
            j--;
        }
        int temp = A[i - 1];
        A[i - 1] = A[j];
        A[j] = temp;
        return A;
    }

    private int binarySearch(int[] nums, int left, int right, int target) {
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] >= target) right = mid;
            else left = mid + 1;
        }
        return left;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：41MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;暂无，密切关注。