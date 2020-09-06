[toc]

Given a square matrix `mat`, return the sum of the matrix diagonals.

Only include the sum of all the elements on the primary diagonal and all the elements on the secondary diagonal that are not part of the primary diagonal.



**Constraints**:

* $n == \text{mat.length} == \text{mat[i].length}$
* $1 \le n \le 100$
* $1 \le \text{mat[i][j]} \le 100$



## 题目解读

&emsp;求正方形对角线元素之和。

```java
class Solution {
    public int diagonalSum(int[][] mat) {

    }
}
```

## 程序设计

* 遍历计算每行的两个对角线元素，注意如果行数是奇数，则两个对角线相交的点多加了一次，需要减去。

```java
class Solution {
    public int diagonalSum(int[][] mat) {
        int n = mat.length, res = 0;
        for (int i = 0; i < n; i++) res += mat[i][i] + mat[i][n - i - 1];
        if (n % 2 == 1) res -= mat[n / 2][n / 2];
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。



## 官方解题

&emsp;