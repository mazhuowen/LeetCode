[toc]

The n-queens puzzle is the problem of placing n queens on an n×n chessboard such that no two queens attack each other.

<img src="../images/#51.png" style="zoom:120%;" />

Given an integer n, return all distinct solutions to the n-queens puzzle.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space respectively.

## 题目解读

&emsp;找出所有$n$皇后可能的位置。

```java
class Solution {
    public List<List<String>> solveNQueens(int n) {

    }
}
```

## 程序设计

* 典型回溯问题。

```java
class Solution {
    public List<List<String>> solveNQueens(int n) {
        List<List<String>> res = new LinkedList<>();
        if (n <= 0) return res;

        // 记录每一行存放的列
        int[] record = new int[n];
        // 皇后存在的列
        boolean[] cols = new boolean[n];
        // 皇后存在的两条对角线
        boolean[] a = new boolean[2 * n - 1], b = new boolean[2 * n - 1];
        // 从第一行开始分配回溯
        queue(0, record, cols, a, b, res);
        return res;
    }

    private void queue(int row, int[] record, boolean[] cols, boolean[] a, boolean[] b, List<List<String>> res) {
        int n = record.length;
        // 在row行col列放置皇后
        for (int col = 0; col < n; col++) {
            // 该位置已有皇后
            if (cols[col] || a[n - 1 - row + col] || b[row + col]) continue;
            // 标记占用
            cols[col] = a[n - 1 - row + col] = b[row + col] = true;
            // 记录位置
            record[row] = col;
            // 找到满足条件的一个解
            if (row == n - 1) {
                List<String> temp = new LinkedList<>();
                for (int i = 0; i < n; i++) {
                    temp.add(productString(n, record[i]));
                }
                res.add(temp);
            }
            // 继续下一行
            else {
                queue(row + 1, record, cols, a, b, res);
            }
            // 遍历结束，恢复状态
            cols[col] = a[n - 1 - row + col] = b[row + col] = false;
        }
    }

    private String productString(int num, int col) {
        char[] c = new char[num];
        for (int i = 0; i < num; i++) {
            c[i] = '.';
        }
        c[col] = 'Q';
        return new String(c);
    }
}
```

## 性能分析

&emsp;粗略分析，时间复杂度为$O(N!)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了98.60%的用户。

内存消耗：42.1MB，在所有java提交中击败了5.01%的用户。

## 官方解题

&emsp;同上。