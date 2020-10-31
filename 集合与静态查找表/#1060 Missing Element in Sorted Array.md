[toc]

Given a sorted array A of **unique** numbers, find the `K-th` missing number starting from the leftmost number of the array.



**Note:**

* $1 \le \text{A.length} \le 50000$
* $1 \le \text{A[i]} \le 1e7$
* $1 \le K \le 1e8$



## 题目解读

&emsp;查找只包含唯一值的数组，缺失的第$k$个元素。

```java
class Solution {
    public int missingElement(int[] nums, int k) {

    }
}
```

## 程序设计

* 使用二分查找实现。

```java
class Solution {
    public int missingElement(int[] nums, int k) {
        if(k <= 0 || nums == null || nums.length == 0) throw new IllegalArgumentException("invalid param");

        int min = nums[0];
        int left = 0, right = nums.length;
        while (left < right) {
            int mid = left + (right - left) / 2;
            // mid前面的区间缺失超过k个值
            if(nums[mid] - min - mid - k >= 0) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return min + left + k - 1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：48.3MB，在所有java提交中击败了33.33%的用户。

## 官方解题

&emsp;还提供了线性扫描版本，但代码实现较为繁杂，不够简洁。