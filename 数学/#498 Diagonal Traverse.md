[toc]

Given a matrix of $M \times N$ elements ($M$ rows, $N$ columns), return all elements of the matrix in diagonal order as shown in the below image.



**Note:**

The total number of elements of the given matrix will not exceed 10,000.



## 题目解读

&emsp;对角线遍历。

```java
class Solution {
    public int[] findDiagonalOrder(int[][] matrix) {

    }
}
```

## 程序设计

* 观察得出规律，行列索引相加是奇数，则斜向下遍历，偶数斜向上遍历。

```java
class Solution {
    public int[] findDiagonalOrder(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return new int[]{};

        int m = matrix.length, n = matrix[0].length;
        int[] res = new int[m * n];
        int i = 0, j = 0, idx = 0;
        while (i != m - 1 || j != n - 1) {
            res[idx++] = matrix[i][j];
            // 向上方向
            if ((i + j) % 2 == 0) {
                // 到达末端，转为向下方向
                if (j == n - 1) {
                    i++;
                } else if (i == 0) {
                    j++;
                }
                // 继续向上比那里
                else {
                    i--;
                    j++;
                }
            }
            // 向下方向
            else {
                // 到达末端，转为向上方向
                if (i == m - 1) {
                    j++;
                } else if (j == 0) {
                    i++;
                } 
                // 继续向下遍历
                else {
                    i++;
                    j--;
                }
            }
        }
        res[idx] = matrix[i][j];
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：3ms，在所有java提交中击败了71.36%的用户。

内存消耗：42.1MB，在所有java提交中击败了73.33%的用户。

## 官方解题

&emsp;思路相似，模拟路线。