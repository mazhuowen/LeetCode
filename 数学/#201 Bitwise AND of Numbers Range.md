[toc]

Given a range `[m, n]` where $0 \le m \le n \le 2147483647$, return the bitwise AND of all numbers in this range, inclusive.



## 题目解读

&emsp;返回范围内数字按位与的值。

```java
class Solution {
    public int rangeBitwiseAnd(int m, int n) {

    }
}
```

## 程序设计

* 最基本的思路是遍历按位与，但是会超时。对于给定区间`[m,n]`间的数字，假设$m$和$n$的最大匹配前缀为$i$，即$i$后不相同；由于数字是连续的，即不相等的部分是$0\dots0$、$0\dots1$、$0\dots10$等，每一位必然出现$0$，从而不相等部分的按位与是$0$，问题转化为求$m$、$n$的前缀。

<img src="..\images\#201.png" style="zoom: 33%;" />

* 可通过左移，直到$m$和$n$相等，此时就是其前缀，然后再右移填充$0$。

```java
class Solution {
    public int rangeBitwiseAnd(int m, int n) {
        int shift = 0;
        while (m < n) {
            m >>= 1;
            n >>= 1;
            shift++;
        }
        return m << shift;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$。

执行用时：8ms，在所有java提交中击败了14.93%的用户。

内存消耗：38.1MB，在所有java提交中击败了75.74%的用户。

## 官方解题

&emsp;官方还提供了`Brian Kernighan`算法。

```java
class Solution {
    public int rangeBitwiseAnd(int m, int n) {
        while (m < n) n &= (n - 1);
        return n;
    }
}
```

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$。

执行用时：6ms，在所有java提交中击败了99.95%的用户。

内存消耗：38.1MB，在所有java提交中击败了65.47%的用户。