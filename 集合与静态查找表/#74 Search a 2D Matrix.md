[toc]

Write an efficient algorithm that searches for a value in an $m \times n$ matrix. This matrix has the following properties:

* Integers in each row are sorted from left to right.
* The first integer of each row is greater than the last integer of the previous row.



## 题目解读

&emsp;给定矩形，行列都是增序，每一行的第一个数大于上一行的最后一个数，找到给定的数。

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        
    }
}
```

## 程序设计

* 矩形的有序性决定了当前元素值大于其坐上所有的元素值。利用有序性，可以从第一行最后一位开始比较查询，大于`target`则向前遍历，小于`target`则向下一行遍历。

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m, n;
        if((m = matrix.length) == 0) {
            return false;
        }
        if((n = matrix[0].length) == 0) {
            return false;
        }
        // 利用矩形的有序性从第一行最后一位查询，最后一位比当前元素大，则向前查询，否则查下一行
        int row = 0, col = n - 1;
        while(row < m && col >= 0) {
            if(matrix[row][col] == target) {
                return true;
            } else if(matrix[row][col] > target) {
                col--;
            } else {
                row++;
            }
        }
        return false;
    }
}
```

* 注意矩形整体是增序的，展开就是增序数组，可以利用二分查找。注意如果将$m*n$个元素从0标记到$m*n - 1$，则每个元素的行索引为$row = mark / n$，列索引为$col = mark \% n$，有了这两个重要的规律，其它流程和二分查找相同。

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m, n;
        if((m = matrix.length) == 0) return false;
        if((n = matrix[0].length) == 0) return false;
        int left = 0, right = m * n - 1;
        while(left <= right) {
            int mid = (left + right) / 2;
            // 规律，其它与二分查找相同
            int row = mid / n;
            int col = mid % n;
            if(target == matrix[row][col]) {
                return true;
            }
            if(target > matrix[row][col]) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return false;
    }
}
```

## 性能分析

&emsp;非二分查找时间复杂度为$O(N + M)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：47.1MB，在所有java提交中击败了5.03%的用户。

&emsp;二分查找时间复杂度为$O(\log_2NM)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：51.3MB，在所有java提交中击败了5.03%的用户。

## 官方解题

&emsp;思路与上类似。