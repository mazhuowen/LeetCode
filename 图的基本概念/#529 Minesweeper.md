[toc]

Let's play the minesweeper game!

You are given a 2D char matrix representing the game board. `'M'` represents an **unrevealed** mine, `'E'` represents an **unrevealed** empty square, `'B'` represents a **revealed** blank square that has no adjacent (above, below, left, right, and all 4 diagonals) mines, digit (`'1'` to `'8'`) represents how many mines are adjacent to this **revealed** square, and finally `'X'` represents a **revealed** mine.

Now given the next click position (row and column indices) among all the **unrevealed** squares (`'M'` or `'E'`), return the board after revealing this position according to the following rules:

* If a mine (`'M'`) is revealed, then the game is over - change it to `'X'`.
* If an empty square (`'E'`) with **no adjacent mines** is revealed, then change it to revealed blank (`'B'`) and all of its adjacent **unrevealed** squares should be revealed recursively.
* If an empty square (`'E'`) with **at least one adjacent mine** is revealed, then change it to a digit (`'1'` to `'8'`) representing the number of adjacent mines.
* Return the board when no more squares will be revealed.



Note:

* The range of the input matrix's height and width is `[1,50]`.
* The click position will only be an unrevealed square (`'M'` or `'E'`), which also means the input board contains at least one clickable square.
* The input board won't be a stage when game is over (some mines have been revealed).
* For simplicity, not mentioned rules should be ignored in this problem. For example, you **don't** need to reveal all the unrevealed mines when the game is over, consider any cases that you will win the game or flag any squares.



## 题目解读

&emsp;给定起始位置，如果挖出地雷，则置为`X`并返回；如果周围存在地雷，则置为地雷数；如果周围不存在地雷，则向四周扩散直到所有边界都是周围存在地雷的空格子。

```java
class Solution {
    public char[][] updateBoard(char[][] board, int[] click) {

    }
}
```

## 程序设计

* 深度优先搜索，终止条件为当前格子周围存在地雷。

```java
class Solution {
    int[] delta = new int[]{1, 0, -1, 0, 1, 1, -1, -1, 1};

    public char[][] updateBoard(char[][] board, int[] click) {
        int i = click[0], j = click[1];
        // 爆炸
        if (board[i][j] == 'M') board[i][j] = 'X';
        else updateBoard(board, i, j);

        return board;
    }

    private void updateBoard(char[][] board, int i, int j) {
        if (i < 0 || i >= board.length || j < 0 || j >= board[0].length
            || board[i][j] != 'E') return;
        // 统计周围地雷数目
        int count = count(board, i, j);
        // 存在地雷，则替换为地雷数并终止该方向深度遍历
        if (count != 0) {
            board[i][j] = (char)(count + '0');
            return;
        }
        // 继续向8个方向遍历
        board[i][j] = 'B';
        for (int k = 0; k < 8; k++) {
            updateBoard(board, i + delta[k], j + delta[k + 1]);
        }
    }

    private int count(char[][] board, int i, int j) {
        int sum = 0;
        for (int k = 0; k < 8; k++) {
            int x = i + delta[k], y = j + delta[k + 1];
            if (x < 0 || x >= board.length || y < 0 || y >= board[0].length || board[x][y] != 'M') continue;
            sum++;
        }
        return sum;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时 :1 ms, 在所有 Java 提交中击败了88.77%的用户

内存消耗 :40.2 MB, 在所有 Java 提交中击败了75.00%的用户

## 官方解题

&emsp;暂无，密切关注。