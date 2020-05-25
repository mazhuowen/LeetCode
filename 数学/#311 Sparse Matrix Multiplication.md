[toc]

Given two `sparse matrices` **A** and **B**, return the result of **AB**.

You may assume that **A**'s column number is equal to **B**'s row number.



## 题目解读

&emsp;稀疏矩阵乘法。

```java
class Solution {
    public int[][] multiply(int[][] A, int[][] B) {

    }
}
```

## 程序设计

* 不考虑稀疏矩阵的性质，普通矩阵乘法如下：

```java
class Solution {
    public int[][] multiply(int[][] A, int[][] B) {
        if (A == null || A.length == 0 || A[0].length == 0 || B == null || B.length == 0 ||
                B[0].length == 0 || A[0].length != B.length) throw new IllegalArgumentException("invalid param");

        int m = A.length, n = B[0].length, l = B.length;
        int[][] res = new int[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < l; j++) {
                for (int k = 0; k < n; k++) {
                    res[i][k] += A[i][j] * B[j][k];
                }
            }
        }
        return res;
    }
}
```

* 考虑到稀疏矩阵的性质，采用稀疏表示来表示矩阵。

```java
class Solution {
    public int[][] multiply(int[][] A, int[][] B) {
        if (A == null || A.length == 0 || A[0].length == 0 || B == null || B.length == 0 ||
                B[0].length == 0 || A[0].length != B.length) throw new IllegalArgumentException("invalid param");

        int m = A.length, n = B[0].length, l = B.length;
        int[][] res = new int[m][n];
        List<Matrix> newA = transfer(A);
        List<Matrix> newB = transfer(B);

        // 对稀疏形式做乘法
        for (Matrix a : newA) {
            for (Matrix b : newB) {
                if (a.j != b.i) continue;
                res[a.i][b.j] += a.val * b.val;
            }
        }

        return res;
    }

    // 将稀疏矩阵转化为稀疏表示
    private List<Matrix> transfer(int[][] nums) {
        List<Matrix> res = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            for (int j = 0; j < nums[0].length; j++) {
                if (nums[i][j] == 0) continue;
                res.add(new Matrix(i, j, nums[i][j]));
            }
        }
        return res;
    }
}

class Matrix {
    int i;
    int j;
    int val;

    Matrix(int i, int j, int val) {
        this.i = i;
        this.j = j;
        this.val = val;
    }
}
```

## 性能分析

&emsp;普通乘法时间复杂度为$O(MNL)$，空间复杂度为$O(MN)$。

执行用时：4ms，在所有java提交中击败了53.33%的用户。

内存消耗：40.2MB，在所有java提交中击败了50.00%的用户。

&emsp;稀疏表示时间复杂度为$O(ML + NL)$，空间复杂度为$O(MN)$。

执行用时：1ms，在所有java提交中击败了82.67%的用户。

内存消耗：40.5MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;暂无，密切关注。