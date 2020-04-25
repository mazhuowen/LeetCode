[toc]

A Tic-Tac-Toe board is given as a string array `board`. Return True if and only if it is possible to reach this board position during the course of a valid tic-tac-toe game.

The `board` is a $3 \times 3$ array, and consists of characters `" "`, `"X"`, and `"O"`.  The `" "` character represents an empty square.

Here are the rules of Tic-Tac-Toe:

* Players take turns placing characters into empty squares (`" "`).
* The first player always places `"X"` characters, while the second player always places `"O"` characters.
* `"X"` and `"O"` characters are always placed into empty squares, never filled ones.
* The game ends when there are 3 of the same (non-empty) character filling any row, column, or diagonal.
* The game also ends if all squares are non-empty.
* No more moves can be played if the game is over.



Note:

* `board` is a length-3 array of strings, where each string `board[i]` has length 3.
* Each `board[i][j]` is a character in the set `{" ", "X", "O"}`.



## 题目解读

&emsp;判断给定的`#`字棋排列是否会出现。第一个人放置`X`，第二个人放置`#`。

```java
class Solution {
    public boolean validTicTacToe(String[] board) {

    }
}
```

## 程序设计

* 首先不管任何情况先手比后手的棋多一个或相等；其次如果先手赢的话，必然比后手多一，后手赢必然与先手棋相等。有了这个思路，对棋进行计数并做是否结束比赛的判断。

```java
class Solution {
    public boolean validTicTacToe(String[] board) {
        // 记录行列及两个对角线，X记为1，O记为-1
        int[] record = new int[8];
        // 总的棋数，最后总为1或0
        int count = 0;
        // 完成标识
        boolean xWin = false, oWin = false;

        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (board[i].charAt(j) == 'X') {
                    count++;
                    if (++record[i] == 3 || ++record[3 + j] == 3) xWin = true;
                    if (i == j && ++record[6] == 3) xWin = true; 
                    if (i + j == 2 && ++record[7] == 3) xWin = true;
                } else if (board[i].charAt(j) == 'O') {
                    count--;
                    if (--record[i] == -3 || --record[3 + j] == -3) oWin = true;
                    if (i == j && --record[6] == -3) oWin = true; 
                    if (i + j == 2 && --record[7] == -3) oWin = true;
                }
            }
        }
        // 不可能两个人都赢
        if (xWin && oWin) return false;
        // 先手赢，必须棋数多一
        if (xWin) return count == 1;
        // 后手赢，必须棋数相等
        if (oWin) return count == 0;
        // 其它情况先手棋与后手棋相等或多一
        return count == 0 || count == 1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.5MB，在所有java提交中击败了14.29%的用户。

## 官方解题

&emsp;思路同上，但是分开多次遍历判断。