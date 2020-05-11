[toc]

Given an array of integers `A` sorted in non-decreasing order, return an array of the squares of each number, also in sorted non-decreasing order.



Note:

* $1 \le \text{A.length} \le 10000$
* $-10000 \le \text{A[i]} \le 10000$
* `A` is sorted in non-decreasing order.



## 题目解读

&emsp;给定有序数组，得到其有序平方数组。

```java
class Solution {
    public int[] sortedSquares(int[] A) {

    }
}
```

## 程序设计

* 寻找正负数切分点，类似归并排序合并。

```java
class Solution {
    public int[] sortedSquares(int[] A) {
        if (A == null || A.length == 0) return A;

        // 查找切分点
        int split = binarySearch(A, 0);
        
        // 归并
        int left = split - 1, right = split;
        int idx = 0;
        int[] res = new int[A.length];
        while (left >= 0 && right < A.length) {
            if (pow(A[left]) < pow(A[right])) res[idx++] = pow(A[left--]);
            else res[idx++] = pow(A[right++]);
        }

        while (left >= 0) {
            res[idx++] = pow(A[left--]);
        }

        while (right < A.length) {
            res[idx++] = pow(A[right++]);
        }

        return res;
    }

    private int binarySearch(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] >= target) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    private int pow(int n) {
        return n * n;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了79.14%的用户。

内存消耗：41.3MB，在所有java提交中击败了10.00%的用户。

## 官方解题

&emsp;同上。社区思路忽略寻找切分点，直接从两侧遍历。

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int length;
        if (nums == null || (length = nums.length) == 0) {
            return new int[0];
        }
        int i = 0;
        int j = length - 1;
        int k = length - 1;
        int[] result = new int[length];
        while (i <= j) {
            int temp;
            if (nums[i] * nums[i] > nums[j] * nums[j]) {
                temp = nums[i] * nums[i];
                i++;
            } else {
                temp = nums[j] * nums[j];
                j--;
            }
            result[k--] = temp;
        }
        return result;
    }
}
```

