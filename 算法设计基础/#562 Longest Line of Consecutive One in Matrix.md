[toc]

Given a 01 matrix M, find the longest line of consecutive one in the matrix. The line could be horizontal, vertical, diagonal or anti-diagonal.

**Hint:** The number of elements in the given matrix will not exceed 10,000.



## 题目解读

&emsp;给定只包含0、1的数组，统计最长的连续1的序列长度，序列可以是行、列、斜对角线、反斜对角线。

```java
class Solution {
    public int longestLine(int[][] M) {

    }
}
```

## 程序设计

* 首先想到的是利用哈希映射每个行、列、对角线最后出现的元素坐标和序列长度，每次遍历结点便更新。

```java
class Solution {
    public int longestLine(int[][] M) {
         if (M == null || M.length == 0) return 0;

         int m = M.length, n = M[0].length;
         // 统计数组，存储最后一个1的坐标、所在连续序列长度
         int[][] rows = new int[m][3];
         int[][] cols = new int[n][3];
         int[][] dia = new int[m + n - 1][3];
         int[][] anti = new int[m + n - 1][3];

         int maxCount = 0;
         for (int i = 0; i < m; i++) {
             for (int j = 0; j < n; j++) {
                 if (M[i][j] == 0) continue;

                 // 存在连续序列，更新计数
                 if (rows[i][2] != 0 && rows[i][1] == j - 1) {
                     rows[i][1] = j;
                     rows[i][2]++;
                 } 
                 // 不存在连续序列
                 else {
                     rows[i] = new int[]{i, j, 1};
                 }
                 // 同行的操作
                 if (cols[j][2] != 0 && cols[j][0] == i - 1) {
                     cols[j][0] = i;
                     cols[j][2]++;
                 } else {
                     cols[j] = new int[]{i, j, 1};
                 }
                 // 同行的操作
                 if (dia[n - 1 + i - j][2] != 0 && dia[n - 1 + i - j][0] == i - 1 && dia[n - 1 + i - j][1] == j - 1) {
                      dia[n - 1 + i - j][0] = i;
                      dia[n - 1 + i - j][1] = j;
                      dia[n - 1 + i - j][2]++;
                 } else {
                     dia[n - 1 + i - j] = new int[]{i, j, 1};
                 }
                 // 同行的操作
                 if (anti[i + j][2] != 0 && anti[i + j][0] == i - 1 && anti[i + j][1] == j + 1) {
                     anti[i + j][0] = i;
                     anti[i + j][1] = j;
                     anti[i + j][2]++;
                 } else {
                    anti[i + j] = new int[]{i, j, 1};
                 }
                 // 在外面更新，避免矩形中只有序列长度为1的序列，造成疏漏
                 maxCount = Math.max(maxCount, rows[i][2]);
                 maxCount = Math.max(maxCount, cols[j][2]);
                 maxCount = Math.max(maxCount, dia[n - 1 + i - j][2]);
                 maxCount = Math.max(maxCount, anti[i + j][2]);
             }
         }
         return maxCount;  
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(M + N)$。

执行用时：24ms，在所有java提交中击败了25.00%的用户。

内存消耗：42.2MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方提供了动态规划的思路。利用动态规划记录每个位置上连续序列长度，这样就需要四个数组记录行列对角线。

```java
class Solution {
    public int longestLine(int[][] M) {
        if (M == null || M.length == 0 || M[0].length == 0) return 0;
        
        int ans = 0;
        // 动态规划数组
        int[][] horizontal = new int[M.length][M[0].length];
        int[][] vertical = new int[M.length][M[0].length];
        int[][] diagonal = new int[M.length][M[0].length];
        int[][] antidiagonal = new int[M.length][M[0].length];
        
        for (int i = 0; i != M.length; ++i) {
            for (int j = 0; j != M[0].length; ++j) {
                if (M[i][j] == 0) {
                    horizontal[i][j] = 0;
                    vertical[i][j] = 0;
                    diagonal[i][j] = 0;
                    antidiagonal[i][j] = 0;
                } else {
                    // 记录更新
                    horizontal[i][j] = j > 0 ? horizontal[i][j - 1] + 1 : 1;
                    vertical[i][j] = i > 0 ? vertical[i - 1][j] + 1 : 1;
                    diagonal[i][j] = i > 0 && j > 0 ? diagonal[i - 1][j - 1] + 1 : 1;
                    antidiagonal[i][j] = i > 0 && j < M[0].length - 1 ? antidiagonal[i - 1][j + 1] + 1 : 1;
                    // 更新结果
                    ans = Math.max(ans, horizontal[i][j]);
                    ans = Math.max(ans, vertical[i][j]);
                    ans = Math.max(ans, diagonal[i][j]);
                    ans = Math.max(ans, antidiagonal[i][j]);
                }
            }
        }
        return ans;
    }
}
```

&emsp;官方还给出了优化后的版本：

```java
class Solution {
    public int longestLine(int[][] M) {
        if (M == null || M.length == 0 || M[0].length == 0) return 0;
        
        int ans = 0;
        // 使用一维数组
        int[] horizontal = new int[M[0].length];
        int[] vertical = new int[M[0].length];
        int[] diagonal = new int[M[0].length];
        int[] antidiagonal = new int[M[0].length];
        
        for (int i = 0; i != M.length; ++i) {
            int[] vertical_new = new int[M[0].length];
            int[] diagonal_new = new int[M[0].length];
            int[] antidiagonal_new = new int[M[0].length];
            for (int j = 0; j != M[0].length; ++j) {
                if (M[i][j] == 0) {
                    horizontal[j] = 0;
                    vertical_new[j] = 0;
                    diagonal_new[j] = 0;
                    antidiagonal_new[j] = 0;
                } else {
                    horizontal[j] = j > 0 ? horizontal[j - 1] + 1 : 1;
                    vertical_new[j] = i > 0 ? vertical[j] + 1 : 1;
                    diagonal_new[j] = i > 0 && j > 0 ? diagonal[j - 1] + 1 : 1;
                    antidiagonal_new[j] = i > 0 && j < M[0].length - 1 ? antidiagonal[j + 1] + 1 : 1;
                    ans = Math.max(ans, horizontal[j]);
                    ans = Math.max(ans, vertical_new[j]);
                    ans = Math.max(ans, diagonal_new[j]);
                    ans = Math.max(ans, antidiagonal_new[j]);
                }
            }
            vertical = vertical_new;
            diagonal = diagonal_new;
            antidiagonal = antidiagonal_new;
        }
        return ans;
    }
}
```

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(N)$。

执行用时：10ms，在所有java提交中击败了88.64%的用户。

内存消耗：44MB，在所有java提交中击败了100.00%的用户。