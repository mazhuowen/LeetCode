[toc]

You are given an $n \times n$ 2D matrix representing an image.

Rotate the image by 90 degrees (clockwise).



**Note**:

You have to rotate the image `in-place`, which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.



## 题目解读

&emsp;顺时针旋转正方形90度。

```java
class Solution {
    public void rotate(int[][] matrix) {
        
    }
}
```

## 程序设计

* 分析规律，正方形顺时针旋转90度，以$4 \times 4$为例，顶点`(0,0)`的值会到`(0,3)`，`(0,3)`的值会到`(3,3)`，`(3,3)`的值会到`(3,0)`；每次旋转实际上需要替换四次，也就是说我们需要$\frac{1}{4}$的点替换四次，完成整个正方形的替换。
* 选择正方形左上角的$\frac{1}{4}$区域，若$n$为偶数，这块区域就是`(0,0)`到`(n/2,n/2)`的区域；若是奇数，则这块区域是`(0,0)`到`(n/2,(n + 1)/2)`。
* 假设这个区域中一点为`(i,j)`，上面分析了定点替换路径为：`(0,0)->(0,3)->(3,3)->(3,0)`；以结点`(0,2)`为例分析，则路径为`(0,2)->(2,3)->(3,1)->(1,0)`，可以归纳出任意结点的替换路径为`(i,j)->(j,n-i-1)->(n-i-1,n-j-1)->(n-j-1,i)`。从而得到代码：

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n;
        if(matrix == null || (n = matrix.length) <= 0) {
            return;
        }
        // 四分之一区域的点进行连续替换
        for(int i = 0; i < n / 2; i++) {
            for(int j = 0; j < (n + 1) / 2; j++) {
                // 连续替换四次
                int temp = matrix[i][j];
                matrix[i][j] = matrix[n - j - 1][i];
                matrix[n - j - 1][i] = matrix[n - i - 1][n - j - 1];
                matrix[n - i - 1][n - j - 1] = matrix[j][n - i - 1];
                matrix[j][n - i - 1]  = temp;
            }
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：42MB，在所有java提交中击败了5.26%的用户。

## 官方解题

&emsp;官方除了上述思路，还提供了一种简单基础的思路，即先根据对角线反转正方形，然后反转每一行，最后得到正确结果。

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        if(n <=1) {
            return;
        }
        // 沿对角线反转正方形（矩形转置切记j=i）
        for(int i = 0; i < n; i++) {
            for(int j = i; j < n; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }
        // 反转每一行
        for(int i = 0; i < n; i++) {
            for(int j =0; j < n/2; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[i ][n - j - 1];
                matrix[i][n - j - 1] = temp;
            }
        }
    }
}
```

> 矩形转置切记j=i开始遍历。

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：42.2MB，在所有java提交中击败了5.26%的用户。

