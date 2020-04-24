[toc]

According to the Wikipedia's article: "The **Game of Life**, also known simply as **Life**, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."

Given a board with $m$ by $n$ cells, each cell has an initial state `live (1)` or `dead (0)`. Each cell interacts with its `eight neighbors` (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):

* Any live cell with fewer than two live neighbors dies, as if caused by under-population.
* Any live cell with two or three live neighbors lives on to the next generation.
* Any live cell with more than three live neighbors dies, as if by over-population..
* Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.

Write a function to compute the next state (after one update) of the board given its current state. The next state is created by applying the above rules simultaneously to every cell in the current state, where births and deaths occur simultaneously.



Follow up:

* Could you solve it in-place? Remember that the board needs to be updated at the same time: You cannot update some cells first and then use their updated values to update other cells.
* In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches the border of the array. How would you address these problems?



## 题目解读

&emsp;生命游戏，是英国数学家约翰·何顿·康威在1970年发明的细胞自动机。给定一个包含$m \times n$个格子的面板，每一个格子都可以看成是一个细胞。每个细胞都具有一个初始状态：$1$即为活细胞（live），或$0$即为死细胞（dead）。每个细胞与其八个相邻位置（水平，垂直，对角线）的细胞都遵循以下四条生存定律：

* 如果活细胞周围八个位置的活细胞数少于两个，则该位置活细胞死亡；
* 如果活细胞周围八个位置有两个或三个活细胞，则该位置活细胞仍然存活；
* 如果活细胞周围八个位置有超过三个活细胞，则该位置活细胞死亡；
* 如果死细胞周围正好有三个活细胞，则该位置死细胞复活；

根据当前状态，计算面板上所有细胞的下一个（一次更新后的）状态。下一个状态是通过将上述规则同时应用于当前状态下的每个细胞所形成的，其中细胞的出生和死亡是同时发生的。

```java
class Solution {
    public void gameOfLife(int[][] board) {

    }
}
```

## 程序设计

* 最基本的思路是复制一个矩形，根据原矩形的数值更新。题目要求不借助额外的空间，观察到格子中的值只能是0或1，可以新增更多的状态，如2表示1变为0，3表示0变为1。

```java
class Solution {
    public void gameOfLife(int[][] board) {
        int m = board.length, n = board[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                board[i][j] = getNewStat(i, j, board);
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 2) board[i][j] = 0;
                else if (board[i][j] == 3) board[i][j] = 1;
            }
        }
    }

    private int getNewStat(int i, int j, int[][] board) {
        // 周围的细胞数
        int sum = 0;
        for (int r = -1; r <= 1; r++) {
            for (int c = -1; c <= 1; c++) {
                // 注意不能重复加当前细胞
                if (i + r < 0 || i + r >= board.length || j + c < 0 || j + c >= board[0].length || (r == 0 && c == 0)) continue;
                sum += getOldStat(board[i + r][j + c]);
            }
        }

        if (board[i][j] == 0) {
            // 复活
            if (sum == 3) return 3;
            // 保持原状
            else return 0;
        } else {
            // 死亡
            if (sum < 2 || sum > 3) return 2;
            // 保持原状
            else return 1; 
        }
    }

    private int getOldStat(int val) {
        // 死亡，原状态为1
        if (val == 2) return 1;
        // 复活，原状态为0
        if (val == 3) return 0;
        return val;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.3MB，在所有java提交中击败了11.11%的用户。

## 官方解题

&emsp;同上。