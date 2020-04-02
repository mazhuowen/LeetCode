[toc]

Nearly every one have used the [Multiplication Table](https://en.wikipedia.org/wiki/Multiplication_table). But could you find out the `k-th` smallest number quickly from the multiplication table?

Given the height `m` and the length `n` of a `m * n` Multiplication Table, and a positive integer `k`, you need to return the `k-th` smallest number in this table.

Note:

* The `m` and `n` will be in the range `[1, 30000]`.
* The `k` will be in the range `[1, m * n]`



## 题目解读

&emsp;给定`m`和`n`的乘法表，找出第`k`小的元素。

```java
class Solution {
    public int findKthNumber(int m, int n, int k) {

    }
}
```

## 程序设计

* 乘法表有个结构特性，即后面的值比前面的大，下面的值比上面大，自然想到有序矩形的遍历；可利用二分查找生成可能的第`k`小的值，然后用有序矩形的遍历来统计验证是否是第`k`小的值。

```java
class Solution {
    public int findKthNumber(int m, int n, int k) {
        if (k <= 0 || m <= 0 || n <= 0 || k > m * n) throw new IllegalArgumentException("invalid param");

        // 由于存在重复，第k小的值必然小于等于k
        int left = 1, right = k;
        while (left < right) {
            int mid = left + (right - left) / 2;
            // 统计小于mid的值是否有k个
            if (count(mid, m, n) >= k) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        // left为大于等于k个值的第一个值，由于肯定存在等于k的值，故left就是要找的数
        return left;
    }

    // 统计小于等于target的数的个数
    private int count(int target, int m, int n) {
        int count = 0;
        // 有序矩形的遍历
        int row = 1, col = n;
        while (row <= m && col >= 0) {
            if (row * col > target) {
                col--;
            } else {
                // 统计当前行小于等于target的个数
                count += col;
                row++;
            }
        }
        return count;
    }
}
```

## 性能分析

&emsp;遍历矩形时间复杂度为$O(M + N)$，总的时间复杂度为$O((M + N)\log_2K)$；空间复杂度为$O(1)$。

执行用时：11ms，在所有java提交中击败了97.15%的用户。

内存消耗：36.2MB，在所有java提交中击败了6.67%的用户。

## 官方解题

&emsp;官方思路同上，在统计上稍有不同。