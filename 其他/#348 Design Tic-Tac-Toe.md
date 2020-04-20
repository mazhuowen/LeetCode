[toc]

Design a Tic-tac-toe game that is played between two players on a $n \times n$ grid.

You may assume the following rules:

* A move is guaranteed to be valid and is placed on an empty block.
* Once a winning condition is reached, no more moves is allowed.
* A player who succeeds in placing n of their marks in a horizontal, vertical, or diagonal row wins the game.

**Follow up:**
Could you do better than $O(n^2)$ per `move()` operation?



## 题目解读

&emsp;井字棋游戏。题目限定每次操作都是合法的。

```java
class TicTacToe {

    /** Initialize your data structure here. */
    public TicTacToe(int n) {

    }
    
    /** Player {player} makes a move at ({row}, {col}).
        @param row The row of the board.
        @param col The column of the board.
        @param player The player, can be either 1 or 2.
        @return The current winning condition, can be either:
                0: No one wins.
                1: Player 1 wins.
                2: Player 2 wins. */
    public int move(int row, int col, int player) {

    }
}

/**
 * Your TicTacToe object will be instantiated and called as such:
 * TicTacToe obj = new TicTacToe(n);
 * int param_1 = obj.move(row,col,player);
 */
```

## 程序设计

* 由于每次操作都合法，无需校验棋的位置，只需记录每行、每列、对角线上棋的数量，如果达到$n$则表示有一方获胜；为了区分两个选手，选手1加一，选手二减一。

```java
class TicTacToe {
    private int n;
    // 行列
    private int[] rows;
    private int[] cols;
    // 对角线及反对角线
    private int[] dias1;
    private int[] dias2;

    public TicTacToe(int n) {
        this.n = n;
        // 选手1放置则加一，选手2放置减一
        this.rows = new int[n];
        this.cols = new int[n];
        this.dias1 = new int[2 * n - 1];
        this.dias2 = new int[2 * n - 1];
    }
    
    public int move(int row, int col, int player) {
        // 用户1操作则加一
        if (player == 1) {
            if (++rows[row] == n || ++cols[col] == n || ++dias1[n - 1 + col - row] == n || ++dias2[col + row] == n) return player;
        }
        // 用户2操作则减一
        else {
            if (--rows[row] == -n || --cols[col] == -n || --dias1[n - 1 + col - row] == -n || --dias2[col + row] == -n) return player;
        }
        return 0;
    }
}
```

## 性能分析

&emsp;单次操作时间复杂度为$O(1)$，空间复杂度为$O(N)$。

执行用时：5ms，在所有java提交中击败了89.29%的用户。

内存消耗：42.3MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。