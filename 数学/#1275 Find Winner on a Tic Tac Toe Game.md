[toc]

Tic-tac-toe is played by two players `A` and `B` on a $3 \times 3$ grid.

Here are the rules of Tic-Tac-Toe:

* Players take turns placing characters into empty squares (" ").
* The first player `A` always places `"X"` characters, while the second player `B` always places `"O"` characters.
* `"X"` and `"O"` characters are always placed into empty squares, never on filled ones.
* The game ends when there are $3$ of the same (non-empty) character filling any row, column, or diagonal.
* The game also ends if all squares are non-empty.
* No more moves can be played if the game is over.

Given an array `moves` where each element is another array of size 2 corresponding to the row and column of the grid where they mark their respective character in the order in which `A` and `B` play.

Return the winner of the game if it exists (`A` or `B`), in case the game ends in a draw return "Draw", if there are still movements to play return "Pending".

You can assume that `moves` is **valid** (It follows the rules of Tic-Tac-Toe), the grid is initially empty and A will play **first**.



**Constraints**:

* $1 \le \text{moves.length} \le 9$
* $\text{moves[i].length} == 2$
* $0 \le \text{moves[i][j]} \le 2$
* There are no repeated elements on `moves`.
* `moves` follow the rules of tic tac toe.



## 题目解读

&emsp;给定井字棋操作顺序，判断哪位选手赢。

```java
class Solution {
    public String tictactoe(int[][] moves) {

    }
}
```

## 程序设计

* 由于是固定长度$3 \times 3$，可使用$9$位二进制数来表示获胜时棋盘摆放状态，则只需遍历操作步骤数组，更新每个选手的棋子状态，和获胜状态做对比。

```java
class Solution {
    // 使用9位二进制数表示凑够三个棋子时的状况
    int[] wins = new int[]{0b111000000, 0b000111000, 0b000000111, 0b100100100, 0b010010010, 0b001001001, 0b100010001, 0b001010100};

    public String tictactoe(int[][] moves) {
        int statA = 0, statB = 0;
        for (int i = 0; i < moves.length; i++) {
            int offset = 1 << (moves[i][0] * 3 + moves[i][1]);
            // A的操作
            if ((i & 1) == 0) {
                statA |= offset;
                if (win(statA)) return "A";
            }
            // B的操作
            else {
                statB |= offset;
                if (win(statB)) return "B";
            }
        }
        return moves.length >= 9 ? "Draw" : "Pending";
    }

    private boolean win(int stat) {
        for (int win : wins) {
            if ((stat & win) == win) return true;
        }
        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(9 \times 8)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.5MB，在所有java提交中击败了52.08%的用户。

## 官方解题

&emsp;官方思路类似。