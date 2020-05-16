[toc]

On a 2 dimensional grid with `R` rows and `C` columns, we start at `(r0, c0)` facing east.

Here, the north-west corner of the grid is at the first row and column, and the south-east corner of the grid is at the last row and column.

Now, we walk in a clockwise spiral shape to visit every position in this grid. 

Whenever we would move outside the boundary of the grid, we continue our walk outside the grid (but may return to the grid boundary later.) 

Eventually, we reach all `R * C` spaces of the grid.

Return a list of coordinates representing the positions of the grid in the order they were visited.

<img src="../images/#885.png" style="zoom:150%;" />



**Note:**

* $1 \le R \le 100$
* $1 \le C \le 100$
* $0 \le r_0 < R$
* $0 \le c_0 < C$



## 题目解读

&emsp;从指定点螺旋输出矩形。

```java
class Solution {
    public int[][] spiralMatrixIII(int R, int C, int r0, int c0) {

    }
}
```

## 程序设计

* 参考[#54 Spiral Matrix](./#54 Spiral Matrix.md)，从内往外模拟旋转路径。

```java
class Solution {
    public int[][] spiralMatrixIII(int R, int C, int r0, int c0) {
        if (R <= 0 || C <= 0) throw new IllegalArgumentException("invalid param");

        int idx = 0;
        int[][] res = new int[R * C][2];
        int rowS = r0, rowE = r0 + 1, colS = c0 - 1, colE = c0 + 1;

        while (idx < R * C) {
            // 向右遍历（注意判断，起始点不包括cols，为rows,cols-1）
            if (r0 == rowS && c0 != colS) {
                if (r0 >= 0 && r0 < R && c0 >= 0 && c0 < C) res[idx++] = new int[]{r0, c0};
                if (c0 == colE) r0++;
                else c0++;
            }
            // 向下遍历
            else if (c0 == colE) {
                // 符合条件则加入数组
                if (r0 >= 0 && r0 < R && c0 >= 0 && c0 < C) res[idx++] = new int[]{r0, c0};
                if (r0 == rowE) c0--;
                else r0++;
            }
            // 向左遍历
            else if (r0 == rowE) {
                if (r0 >= 0 && r0 < R && c0 >= 0 && c0 < C) res[idx++] = new int[]{r0, c0};
                if (c0 == colS) r0--;
                else c0--;
            }
            // 向上遍历
            else if (c0 == colS) {
                if (r0 >= 0 && r0 < R && c0 >= 0 && c0 < C) res[idx++] = new int[]{r0, c0};
                if (r0 == rowS) {
                    rowS--; rowE++; colS--; colE++;
                }
                r0--;
            }
        }
       
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\max(M,N)^2)$，空间复杂度为$O(MN)$。

执行用时：5ms，在所有java提交中击败了76.74%的用户。

内存消耗：41MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;官方提供思路类似。