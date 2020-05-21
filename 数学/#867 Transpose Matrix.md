[toc]

Given a matrix A, return the transpose of A.

The transpose of a matrix is the matrix flipped over it's main diagonal, switching the row and column indices of the matrix.



**Note:**

* $1 \le \text{A.length} \le 1000$

* $1 \le \text{A[0].length} \le 1000$



## 题目解读

&emsp;实现矩阵的转置。

```java
class Solution {
    public int[][] transpose(int[][] A) {
    
    }
}
```

## 程序设计

* 矩阵遍历。

```java
class Solution {
    public int[][] transpose(int[][] A) {
        if (A == null || A.length == 0 || A[0].length == 0) throw new IllegalArgumentException("invalid param");

        int m = A.length, n = A[0].length;
        int[][] res = new int[n][m];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                res[j][i] = A[i][j];
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：41MB，在所有java提交中击败了20.00%的用户。

## 官方解题

&emsp;同上。