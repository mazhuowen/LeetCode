[toc]

Given an $m \times n$ matrix `M` initialized with all `0`'s and several update operations.

Operations are represented by a 2D array, and each operation is represented by an array with two **positive** integers `a` and `b`, which means `M[i][j]` should be added by one for all $0 \le i < a$ and $0 \le j < b$.

You need to count and return the number of maximum integers in the matrix after performing all the operations.

Note:

* The range of `m` and `n` is `[1,40000]`.
* The range of `a` is `[1,m]`, and the range of `b` is `[1,n]`.
* The range of operations size won't exceed `10000`.



## 题目解读

&emsp;统计操作之后最大数字的数目。

```java
class Solution {
    public int maxCount(int m, int n, int[][] ops) {

    }
}
```

## 程序设计

* 操作数多的区间数值大，问题转化为求这些操作重叠的区间大小。

```java
class Solution {
    public int maxCount(int m, int n, int[][] ops) {
        if (ops == null) return m * n;

        int minX = m, minY = n;
        for (int[] op : ops) {
            minX = Math.min(minX, op[0]);
            minY = Math.min(minY, op[1]);
        }
        // 所有操作重叠的区间
        return minX * minY;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(L)$，空间复杂度为$O(1)$，$L$为操作数长度。

执行用时：1ms，在所有java提交中击败了76.89%的用户。

内存消耗：39.6MB，在所有java提交中击败了33.33%的用户。

## 官方解题

&emsp;同上。