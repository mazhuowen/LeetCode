[toc]

Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

* Integers in each row are sorted in ascending from left to right.
* Integers in each column are sorted in ascending from top to bottom.



## 题目解读

&emsp;给定矩阵行列有序，查找指定的元素是否在矩阵中。

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        
    }
}
```

## 程序设计

* 可以利用结构特性从第一行最大值查找。

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0) return false;

        int m = matrix.length, n = matrix[0].length;
        int baseRow = 0, baseCol = n - 1;
        while (baseRow >= 0 && baseRow < m && baseCol >= 0 && baseCol < n) {
            if (matrix[baseRow][baseCol] == target) return true;

            if (matrix[baseRow][baseCol] > target) baseCol--;
            else baseRow++;
        }
        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(M + N)$，空间复杂度为$O(1)$。

执行用时：6ms，在所有java提交中击败了99.75%的用户。

内存消耗：45.5MB，在所有java提交中击败了14.85%的用户。

## 官方解题

&emsp;同上。