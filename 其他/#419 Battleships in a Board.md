[toc]

Given an 2D board, count how many battleships are in it. The battleships are represented with `'X'`s, empty slots are represented with `'.'`s. You may assume the following rules:

* You receive a valid board, made of only battleships or empty slots.
* Battleships can only be placed horizontally or vertically. In other words, they can only be made of the shape $1 \times N$ (1 row, N columns) or $N \times 1$ (N rows, 1 column), where N can be of any size.
* At least one horizontal or vertical cell separates between two battleships - there are no adjacent battleships.



**Follow up**:
Could you do it in **one-pass**, using only **$O(1)$ extra memory** and **without modifying** the value of the board?



## 题目解读

&emsp;战列舰只能横放或竖放，两个战列舰不能相邻，必须有一个垂直或水平的空格。统计战列舰的数量。

```java
class Solution {
    public int countBattleships(char[][] board) {

    }
}
```

## 程序设计

* 由于题目保证输入是有效的，每条战列舰要么水平要么垂直放置，每条战列舰和其他船只都有空格。可以一次扫描，只计数船首，由于战舰相隔，与`X`相邻的`X`必然是同一条战舰。

```java
class Solution {
    public int countBattleships(char[][] board) {
        if (board == null || board.length == 0) return 0;
        int m = board.length, n = board[0].length;

        int count = 0;
        // 首先假设水平划分
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == '.') continue;

                // 检查左侧是否是X，是则说明是水平划分的同一条战列舰，不是则计数加一
                // 检查上方是否是X，是则说明是垂直划分的同一条战列舰，不是则计数加一
                if ((j == 0 || board[i][j - 1] == '.') && (i == 0 || board[i - 1][j] == '.')) count++;
            }
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。