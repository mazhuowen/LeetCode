[toc]

Given a m x n matrix, if an element is 0, set its entire row and column to 0. Do it in-place.



**Follow up**:

* A straight forward solution using $O(mn)$ space is probably a bad idea.
* A simple improvement uses $O(m + n)$ space, but still not the best solution.
* Could you devise a constant space solution?



## 题目解读

&emsp;将矩形中位置元素为$0$的行列置为$0$。

```java
class Solution {
    public void setZeroes(int[][] matrix) {

    }
}
```

## 程序设计

* 最基本的思路是额外空间保存为$0$的行列，然后重置。

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        if (matrix == null) return;
        int m = matrix.length, n = matrix[0].length;

        boolean[] rows = new boolean[m], cols = new boolean[n];

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    rows[i] = true;
                    cols[j] = true;
                }
            }
        }
        for (int i = 0; i < m; i++) {
            if (rows[i]) {
                for (int j = 0; j < n; j++) matrix[i][j] = 0;
            }
        }
        for (int j = 0; j < n; j++) {
            if (cols[j]) {
                for (int i = 0; i < m; i++) matrix[i][j] = 0;
            }
        }
    }
}
```

* 官方还提出暴力法，将存在0的行列元素标记为其相反数，然后再次将负数置为0。除了暴力法，仔细观察，如果能将存在0的行列标记在矩形中，就可以不用额外空间；遍历是自顶向下，从左至右，如果遇到为0的元素，可以将行首和列首置为0，这样不影响后续遍历，然后再重置矩形。
* 需注意首行和首列的标识都会放到`(0,0)`索引下，会发生覆盖，需要额外的变量记录首行或首列的标识。

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        if (matrix == null) return;
        int m = matrix.length, n = matrix[0].length;

        boolean firstCol = false;
        for (int i = 0; i < m; i++) {
            // 第一列需要标识，避免覆盖第一行信息
            if (matrix[i][0] == 0) firstCol = true;
            // 从第二列开始
            for (int j = 1; j < n; j++) {
                if (matrix[i][j] == 0) {
                    // 标记
                   matrix[i][0] = 0;
                   matrix[0][j] = 0;
                }
            }
        }
        // 重置
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) matrix[i][j] = 0;
            }
        }
        // 判断第一行
        if (matrix[0][0] == 0) {
            for (int j = 0; j < n; j++) matrix[0][j] = 0;
        }
        // 第一列是否需要重置
        if (firstCol) {
            for (int i = 0; i < m; i++) matrix[i][0] = 0;
        }
    }
}
```

## 性能分析

&emsp;基本思路时间复杂度为$O(MN)$，空间复杂度为$O(M + N)$。

执行用时：1ms，在所有java提交中击败了99.97%的用户。

内存消耗：41MB，在所有java提交中击败了100.00%的用户。

&emsp;改进方法时间复杂度为$O(MN)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了99.97%的用户。

内存消耗：41MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;参考官方思路。