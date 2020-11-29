[toc]

Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy **all of the following rules**:

* Each of the digits `1-9` must occur exactly once in each row.
* Each of the digits `1-9` must occur exactly once in each column.
* Each of the the digits `1-9` must occur exactly once in each of the 9 $3 \times 3$ sub-boxes of the grid.

Empty cells are indicated by the character '.'.



**Note**:

* The given board contain only digits `1-9` and the character `'.'`.
* You may assume that the given Sudoku puzzle will have a single unique solution.
* The given board size is always $9 \times 9$.



## 题目解读

&emsp;解数独。

<img src="../images/#37.png"  />

```java
class Solution {
    public void solveSudoku(char[][] board) {

    }
}
```

## 程序设计

* 回溯，如果成功则直接返回，失败继续尝试。

```java
class Solution {
    // 记录行、列、方格中数字填充情况（位置0空置）
    boolean[][] rows = new boolean[9][10];
    boolean[][] cols = new boolean[9][10];
    boolean[][] grids = new boolean[9][10];

    public void solveSudoku(char[][] board) {
        // 初始化填充情况
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                if (board[i][j] == '.') continue;
                char c = board[i][j];
                rows[i][c - '0'] = true;
                cols[j][c - '0'] = true;
                grids[i / 3 * 3 + j / 3][c - '0'] = true;
            }
        }
        solveSudoku(board, 0);
    }

    private boolean solveSudoku(char[][] board, int idx) {
        // 找到可填充点
        while (idx < 81 && board[idx / 9][idx % 9] != '.') idx++;
        // 填充结束
        if (idx == 81) return true;

        int row = idx / 9, col = idx % 9, gridIdx = row / 3 * 3 + col / 3;
        // 尝试填充
        for (int i = 1; i < 10; i++) {
            //已填充，跳过
            if (rows[row][i] || cols[col][i] || grids[gridIdx][i]) continue;
            // 尝试
            board[row][col] = (char)(i + '0');
            rows[row][i] = cols[col][i] = grids[gridIdx][i] = true;
            // 成功则直接返回
            if (solveSudoku(board, idx + 1)) return true;
            // 回溯
            board[row][col] = '.';
            rows[row][i] = cols[col][i] = grids[gridIdx][i] = false;
        }
        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O((9!)^{9})$，空间复杂度为$O(1)$。

执行用时：3ms，在所有java提交中击败了92.66%的用户。

内存消耗：37.4MB，在所有java提交中击败了28.57%的用户。

## 官方解题

&emsp;同上。