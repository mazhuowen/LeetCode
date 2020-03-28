[toc]

he n-queens puzzle is the problem of placing n queens on an $n \times n$ chessboard such that no two queens attack each other.

Given an integer n, return the number of distinct solutions to the n-queens puzzle.



## 题目解读

&emsp;$n$皇后问题的简化。

```java
class Solution {
    public int totalNQueens(int n) {

    }
}
```

## 程序设计

* $n$皇后问题的简化。

```java
class Solution {
    int count = 0;

    public int totalNQueens(int n) {
        if (n <= 0) return 0;

        // 皇后存在的列
        boolean[] cols = new boolean[n];
        // 皇后存在的两条对角线
        boolean[] a = new boolean[2 * n - 1], b = new boolean[2 * n - 1];
        // 从第一行开始分配回溯
        queue(0, cols, a, b);
        return count;
    }

    private void queue(int row, boolean[] cols, boolean[] a, boolean[] b) {
        int n = cols.length;
        // 在row行col列放置皇后
        for (int col = 0; col < n; col++) {
            // 该位置已有皇后
            if (cols[col] || a[n - 1 - row + col] || b[row + col]) continue;
            
            // 标记占用
            cols[col] = a[n - 1 - row + col] = b[row + col] = true;
            // 找到满足条件的一个解
            if (row == n - 1) {
                count++;
            }
            // 继续下一行
            else {
                queue(row + 1, cols, a, b);
            }
            // 遍历结束，恢复状态
            cols[col] = a[n - 1 - row + col] = b[row + col] = false;
        }
    }
}
```

## 性能分析

&emsp;粗略分析时间复杂度为$O(N!)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了88.35%的用户。

内存消耗：36.3MB，在所有java提交中击败了5.20%的用户。

## 官方解题

&emsp;思路同上。