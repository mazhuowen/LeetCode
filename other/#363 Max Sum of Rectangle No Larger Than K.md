[toc]

Given a non-empty 2D matrix matrix and an integer $k$, find the max sum of a rectangle in the matrix such that its sum is no larger than $k$.

Note:

* The rectangle inside the matrix must have an area > 0.
* What if the number of rows is much larger than the number of columns?



## 题目解读

&emsp;给定二维数组和数字$k$，在数组中找出元素之和不大于$k$的最大值。题目限定元素中至少有一个正数。

```java
class Solution {
    public int maxSumSubmatrix(int[][] matrix, int k) {
        
    }
}
```

## 程序设计

* 最容易想到的是暴力解法：遍历数组中的每一个结点`x`，然后计算以该结点为右下角结点的所有矩形组合的面积，具体的，计算`x`到其所在行行首`a1`的面积，然后向上扩展，计算上一行的面积并叠加，直到计算完第一行；然后从行首位置开始遍历到下一个结点`a2`，计算`a2`到`x`的面积，同上，向上扩展；直到遍历到结点`x`的前一个结点，向上扩展，完成所有的计算。
* 在每次面积计算中，根据条件更新最大面积值。为了方便计算每一行的面积，且可以向上扩展，需要引入前缀和数组，记录每个结点到行首的面积。

```java
class Solution {
    public int maxSumSubmatrix(int[][] matrix, int k) {
        if(matrix == null || matrix[0] == null) {
            return 0;
        }
        // 记录当前点到行首的面积
        int[][] rowSum = new int[matrix.length][matrix[0].length];
        int maxArea = Integer.MIN_VALUE;
        // 遍历每个点
        for(int row = 0; row < matrix.length; row++) {
            for(int col = 0; col < matrix[0].length; col++) {
                // 计算rowSum
                if(col == 0) {
                    rowSum[row][col] = matrix[row][col];
                } else {
                    rowSum[row][col] = rowSum[row][col - 1] + matrix[row][col];
                }
                
                int tempArea;
                // 对preSum向右遍历
                for(int j = -1; j < col; j++) {
                    // 遍历到不同的列值，也就是不同的矩形的宽，原先的面积清零
                    tempArea = 0;
                    // 向上遍历，整个循环计算当前点(row,j)到(row,col)为底，向上扩展的矩形的界面积
                    for(int i = row; i >= 0; i--) {
                        // 计算截至col的整行的面积
                        if(j == -1) {
                            tempArea += rowSum[i][col];
                        } 
                        // 计算j到col为底边的面积
                        else {
                            tempArea += rowSum[i][col] - rowSum[i][j];
                        }
                        // 符合条件则更新
                        if(tempArea > maxArea && tempArea <= k) {
                            maxArea = tempArea;
                        }
                    }
                }
            }
        }
        return maxArea;
    }
}
```

## 性能分析

&emsp;暴力法时间复杂度为$O(N^2M^2)$，空间复杂度为$O(NM)$。

执行用时：139ms，在所有java提交中击败了68.52%的用户。

内存消耗：43.6MB，在所有java提交中击败了78.75%的用户。

## 官方解题

&emsp;暂无，密切关注。社区较通用的思路实际上仍然是暴力法的改进，迭代行，将当前行及其后面的行压缩为一行，根据压缩的这一行来计算矩形面积。

```java
class Solution {
    public int maxSumSubmatrix(int[][] matrix, int k) {
        if(matrix == null || matrix[0] == null) {
            return 0;
        }
        int maxArea = Integer.MIN_VALUE;
        int row = matrix.length;
        int col = matrix[0].length;
        // 将i行及以下的行压缩为一行
        for(int i = 0; i < row; i++) {
            int[] compressRow = new int[col];
            // 每压缩一行，计算每列的面积并更新
            for(int j = i; j < row; j++) {
                // 此处计算的tempArea是最左边边界起始的i~j行组成的矩形，不包含所有情况
                int tempArea = 0;
                for(int z = 0; z < col; z++) {
                    // 当前行的列值加上历史列值，得到当前列的历史值
                    compressRow[z] += matrix[j][z];
                    tempArea += compressRow[z];
                    // 如果已经满足，则直接返回
                    if(tempArea == k) {
                        return k;
                    } else if(tempArea > maxArea && tempArea < k) {
                        maxArea = tempArea;
                    }
                }
                // 得到压缩数组后，计算边界不是左边的情况，即边界从m开始的任意矩形
                for(int m = 1; m < col; m++) {
                    // 每开始一个新的边界，重置临时面积
                    tempArea = 0;
                    for(int n = m; n < col; n++) {
                        tempArea += compressRow[n];
                        // 如果已经满足，则直接返回
                        if(tempArea == k) {
                            return k;
                        } else if(tempArea > maxArea && tempArea < k) {
                            maxArea = tempArea;
                        }
                    }
                }
            }

        }
        return maxArea;
    }
}
```

&emsp;上述压缩行，最好的情况为$O(N^2M)$，即最大面积等于$k$且存在于最左边界开始的矩形，不走底下的两层循环；最坏情况，走底下的两层循环，时间复杂度为$O(N^2M^2)$。空间复杂度为$O(M)$。

执行用时：83ms，在所有java提交中击败了96.30%的用户。

内存消耗：43.1MB，在所有java提交中击败了88.75%的用户。

&emsp;参考社区中运行时间较快的代码，其在上述逻辑中增加了关键一步，即在最外层两个循环中的第一个循环中，不再记录整个的矩形面积，而是记录整个区域中面积的最大值，如果面积最大值小于等于$k$，则不必走下面的两层循环，因为这个区域其它矩形面积不超过这个最大面积，此时可以省去两层循环，加入这个逻辑，时间性能能达到18ms；第二个措施是从列开始压缩，这样时间性能达到3~5ms。

```java
class Solution {
    public int maxSumSubmatrix(int[][] matrix, int k) {
        if(matrix == null || matrix[0] == null) {
            return 0;
        }
        int maxArea = Integer.MIN_VALUE;
        int row = matrix.length;
        int col = matrix[0].length;
        // 将i列及以后的列压缩为一列
        for(int i = 0; i < col; i++) {
            int[] compressCol = new int[row];
            // 每压缩一列，计算最大面积
            for(int j = i; j < col; j++) {
                // localMax记录在宽为i～j，高为上下边界的区域最大的矩形面积
                int tempArea = 0, localMax = Integer.MIN_VALUE;
                for(int z = 0; z < row; z++) {
                    // 当前列的行值加上历史列值，得到当前列的历史值
                    compressCol[z] += matrix[z][j];
                    // 小于0，则不可能是最大矩形面积的一部分，清零
                    if(tempArea < 0) {
                        tempArea = 0;
                    }
                    tempArea += compressCol[z];
                    // 更新最大矩形面积
                    if(tempArea > localMax) {
                        localMax = tempArea;
                    }
                }
                 // 矩形区域最大面积满足k的要求，则不必走下面的两层for循环
                if(localMax == k) {
                    return k;
                } else if(localMax < k) {
                    maxArea = localMax > maxArea ? localMax : maxArea;
                    // 不必走以下循环
                    continue;
                }
                // 遍历计算矩形面积
                for(int m = 0; m < row; m++) {
                    // 每开始一个新的边界，重置临时面积
                    tempArea = 0;
                    for(int n = m; n < row; n++) {
                        tempArea += compressRow[n];
                        // 如果已经满足，则直接返回
                        if(tempArea == k) {
                            return k;
                        } else if(tempArea > maxArea && tempArea < k) {
                            maxArea = tempArea;
                        }
                    }
                }
            }

        }
        return maxArea;
    }
}
```
执行用时：5ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.3MB，在所有java提交中击败了99.38%的用户。

> 第一个改进性能好只不过是因为测试用例较少，加入上述逻辑大多免去了内层循环，从而大幅度提升运行时间；第二个改进时间少是因为测试用例列小于行，这样加上不走内层循环，最好情况时间复杂度就是$O(M^2N)$，比原始算法最好情况的$O(N^2M)$快。
>
> 需注意这些时间性能差异只是测试用例不均引起，算法本质思想是一样的。其次对于note中的第二个问题，列压缩和行压缩的选择，在这个例子中有了很好的体现。

