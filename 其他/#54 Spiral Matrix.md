[toc]

Given a matrix of $m \times n$ elements (m rows, n columns), return all elements of the matrix in spiral order.



## 题目解读

&emsp;陀螺旋转输出矩形元素。

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        
    }
}
```

## 程序设计

* 仔细观察实际上可以把陀螺旋转的问题拆分为顺时针遍历矩形最外层的问题，关键点就是结束点，即起始点的下方结点，当到达结束点，表示这一轮遍历完成，此时收缩矩形区域，进行下一轮遍历。

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> res = new LinkedList<>();
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return res;
        int m = matrix.length, n = matrix[0].length;
        
        // 矩形区域
        int rowS = 0, rowE = m - 1, colS = 0, colE = n - 1;

        int i = 0, j = 0;
        for (int k = 0; k < m * n; k++) {
            res.add(matrix[i][j]);
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

        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.2MB，在所有java提交中击败了5.72%的用户。

## 官方解题

&emsp;官方提供方法引入额外的空间。