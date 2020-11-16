[toc]

Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner `(row1, col1)` and lower right corner `(row2, col2)`.



**Note**:

* You may assume that the matrix does not change.
* There are many calls to sumRegion function.
* You may assume that $\text{row1} \le \text{row2}$ and $\text{col1} \le \text{col2}$.



## 题目解读

&emsp;求指定区域内元素和。

```java
class NumMatrix {

    public NumMatrix(int[][] matrix) {

    }
    
    public int sumRegion(int row1, int col1, int row2, int col2) {

    }
}

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix obj = new NumMatrix(matrix);
 * int param_1 = obj.sumRegion(row1,col1,row2,col2);
 */
```

## 程序设计

* 由数组前缀和联想到，可以计算每个顶点到左上角区域的和。

```java
class NumMatrix {
    int m, n;
    int[][] preSum;

    public NumMatrix(int[][] matrix) {
        // 矩形不符合要求的特殊情况处理
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            this.m = -1;
            this.n = -1;
            return;
        }
        // 计算前缀和
        this.m = matrix.length;
        this.n = matrix[0].length;
        preSum = new int[m + 1][n + 1];
        int[] rowSum = new int[n + 1];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // 行的前缀和
                rowSum[j + 1] = rowSum[j] + matrix[i][j];
                // 每个点元素和为上一个点元素和加当前行前缀和及本身
                preSum[i + 1][j + 1] = preSum[i][j + 1] + rowSum[j] + matrix[i][j];
            }
        }
    }

    public int sumRegion(int row1, int col1, int row2, int col2) {
        // 坐标不符合要求
        if (row1 >= m || row1 < 0 || row2 >= m || row2 < 0) return 0;
        if (col1 >= n || col1 < 0 || col2 >= n || col2 < 0) return 0;
        // 减去矩形左侧和上方元素和，加上重复减掉的左上角区域
        return preSum[row2 + 1][col2 + 1] - preSum[row2 + 1][col1] - preSum[row1][col2 + 1] + preSum[row1][col1];
    }
}
```

## 性能分析

&emsp;构建时间复杂度为$O(MN)$，空间复杂度为$O(MN)$；计算时间复杂度为$O(1)$。

执行用时：15ms，在所有java提交中击败了97.99%的用户。

内存消耗：45.6MB，在所有java提交中击败了33.33%的用户。

## 官方解题

&emsp;同上。