[toc]

You are given two arrays `rowSum` and `colSum` of non-negative integers where `rowSum[i]` is the sum of the elements in the `i`th row and `colSum[j]` is the sum of the elements of the `j`th column of a 2D matrix. In other words, you do not know the elements of the matrix, but you do know the sums of each row and column.

Find any matrix of **non-negative** integers of size `rowSum.length x​ colSum.length` that satisfies the `rowSum` and `colSum` requirements.

Return a 2D array representing **any** matrix that fulfills the requirements. It's guaranteed that **at least one** matrix that fulfills the requirements exists.

 

**Constraints**:

* $1 \le \text{rowSum.length, colSum.length} \le 500$
* $0 \le \text{rowSum[i], colSum[i]} \le 10^8$
* $\text{sum(rows)} == \text{sum(columns)}$



## 题目解读

&emsp;求得任意一个满足给定的行、列和的非负数矩阵。

```java
class Solution {
    public int[][] restoreMatrix(int[] rowSum, int[] colSum) {

    }
}
```

## 程序设计

* 由于矩阵规模较大，无法使用回溯尝试；转换思路，如果每个格子直接填充行、列中较小的值，剩余的行、列填充$0$，以第一行为例，假设`rowSum`值较小，则在`(0,0)`位置填充该值，`colSum`减去该值，从而问题转化为$n - 1 \times m$的矩阵填充问题，依此类推直到最后一个格子。
* 以行`[27,15,2,14,1,28]`，列`[51,12,0,24]`为例，如下，每次选择较小值，第一行第一列选择$27$，此时该行其他值为$0$，行数下移，第二行、第三行同理；第四行第一列选择较小值，列中剩余的$7$，此时该列其他值为$0$，列右移；依次类推得到剩余值。

<img src="..\images\#1605.png" style="zoom:100%;" />

```java
class Solution {
    public int[][] restoreMatrix(int[] rowSum, int[] colSum) {
        int n = rowSum.length, m = colSum.length;
        int[][] matrix = new int[n][m];
        int row = 0, col = 0;
        while (row < n && col < m) {
            // 行较少，赋值并更新行数
            if (rowSum[row] <= colSum[col]) {
                matrix[row][col] = rowSum[row];
                colSum[col] -= rowSum[row++];
            } 
            // 列较少，赋值并更新列数
            else {
                matrix[row][col] = colSum[col];
                rowSum[row] -= colSum[col++];
            }
        }
        return matrix;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N + M)$，空间复杂度为$O(NM)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：46.9MB，在所有java提交中击败了60.92%的用户。

## 官方解题

&emsp;暂无，密切关注。