[toc]

Given a positive integer $n$, generate a square matrix filled with elements from $1$ to $n^2$ in spiral order.



## 题目解读

&emsp;给定正方形边长，返回对应的螺旋矩阵。

```java
class Solution {
    public int[][] generateMatrix(int n) {

    }
}
```

## 程序设计

* [#54 Spiral Matrix](./#54 Spiral Matrix.md)的变形。

```java
class Solution {
    public int[][] generateMatrix(int n) {
        if (n <= 0) throw new IllegalArgumentException("invalid param");

        int[][] matrix = new int[n][n];
        int val = 1;
        int i = 0, j = 0;
        int rowS = 0, colS = 0, rowE = n - 1, colE = n - 1;

        for (int k = 0; k < n * n; k++) {
            matrix[i][j] = val++;

            // 在第一行，向右遍历
            if (i == rowS) {
                 // 拐角
                if (j == colE) i++;
                else j++;
            }
            // 在最后一列，向下遍历
            else if (j == colE) {
                if (i == rowE) j--;
                else i++;
            }
            // 在最后一行，向右遍历
            else if (i == rowE) {
                if (j == colS) i--;
                else j--; 
            }
            // 在第一列，向上遍历
            else {
                // 结束点，表示当前区域外层已遍历完，缩小区域，定位到新的起始点
                if (i == rowS + 1) {
                    rowS++; colS++; rowE--; colE--;
                    j++;
                } else i--;
            }
        }
        return matrix;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.5MB，在所有java提交中击败了7.14%的用户。

## 官方解题

&emsp;暂无，社区有非常简介的版本：

```java
class Solution {
    public int[][] generateMatrix(int n) {
        int l = 0, r = n - 1, t = 0, b = n - 1;
        int[][] mat = new int[n][n];
        int num = 1, tar = n * n;
        while(num <= tar){
            for(int i = l; i <= r; i++) mat[t][i] = num++; // left to right.
            t++;
            for(int i = t; i <= b; i++) mat[i][r] = num++; // top to bottom.
            r--;
            for(int i = r; i >= l; i--) mat[b][i] = num++; // right to left.
            b--;
            for(int i = b; i >= t; i--) mat[i][l] = num++; // bottom to top.
            l++;
        }
        return mat;
    }
}
```