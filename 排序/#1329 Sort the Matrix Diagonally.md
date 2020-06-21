[toc]

Given a $m \times n$ matrix mat of integers, sort it diagonally in ascending order from the top-left to the bottom-right then return the sorted array.



Constraints:

* $m == \text{mat.length}$
* $n == \text{mat[i].length}$
* $1 \le m, n \le 100$
* $1 \le \text{mat[i][j]} \le 100$



## 题目解读

&emsp;排序矩形对角线的数值。

```java
class Solution {
    public int[][] diagonalSort(int[][] mat) {
        
    }
}
```

## 程序设计

* 最基本的思路是先将对角线元素拼接，然后排序，最后再赋值返回。

```java
class Solution {
    public int[][] diagonalSort(int[][] mat) {
        if (mat == null || mat.length == 0 || mat[0].length == 0) return mat;

        int m = mat.length, n = mat[0].length;
        // 将对角线元素拼接到数组
        List<Integer>[] temp = new ArrayList[m + n - 1];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (temp[j - i + m - 1] == null) temp[j - i + m - 1] = new ArrayList<>();

                temp[j - i + m - 1].add(mat[i][j]);
            }
        }

        // 排序
        for (List<Integer> diag : temp) {
            Collections.sort(diag);
        }

        // 赋值
        int[][] res = new int[m][n];
        for (int i = 0; i < m + n - 1; i++) {
            // 根据对角线编号计算第一个元素的坐标
            int row, col;
            if (i - m + 1 > 0) {
                row = 0;
                col = i - m + 1;
            } else {
                row = m - 1 - i;
                col = 0;
            }
            List<Integer> diag = temp[i];
            for (int val : diag) {
                res[row++][col++] = val;
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O（(M + N)\min(M,N)\log_2(M,N)）$，空间复杂度为$O(MN)$。

执行用时：11ms，在所有java提交中击败了41.19%的用户。

内存消耗：40.5MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区时间性能较好的方法，重写快速排序，在数组内交换。

```
class Solution {
    public int[][] diagonalSort(int[][] mat) {
        if (mat == null || mat.length == 0 || mat[0].length == 0) return mat;

        int m = mat.length, n = mat[0].length;
        
        // 对每个对角线快速排序
        for (int i = 0; i < m; i++) {
            int endX = Math.min(m - 1, i + n - 1);
            int endY = Math.min(n - 1, m - i - 1);
            quickSort(mat, i, 0, endX, endY);
        }

        for (int i = 1; i < n; i++) {
            int endX = Math.min(n - i - 1, m - 1);
            int endY = Math.min(n - 1, i + m - 1);
            quickSort(mat, 0, i, endX, endY);
        }

        return mat;
    }

    private void quickSort(int[][] mat, int startX, int startY, int endX, int endY) {
        if (startX >= endX) return;

        int base = mat[startX][startY];
        int leftX = startX, leftY = startY, rightX = endX, rightY = endY;
        while (leftX < rightX) {
            while (leftX < rightX && mat[rightX][rightY] >= base) {
                rightX--;
                rightY--;
            }
            if (leftX < rightX) swap(mat, leftX++, leftY++, rightX, rightY);

            while (leftX < rightX && mat[leftX][leftY] <= base) {
                leftX++;
                leftY++;
            }
            if (leftX < rightX) swap(mat, leftX, leftY, rightX--, rightY--);
        }
        mat[leftX][leftY] = base;
        quickSort(mat, startX, startY, leftX - 1, leftY - 1);
        quickSort(mat, leftX + 1, leftY + 1, endX, endY);
    }

    private void swap(int[][] mat, int x, int y, int i, int j) {
        int temp = mat[x][y];
        mat[x][y] = mat[i][j];
        mat[i][j] = temp;
    }
}
```

&emsp;时间复杂度为$O((M + N)\min(M,N)\log_2(M,N))$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了91.76%的用户。

内存消耗：40.5MB，在所有java提交中击败了100.00%的用户。