[toc]

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

Constraints:

board and word consists only of lowercase and uppercase English letters.

* $1 \le \text{board.length} \le 200$
* $1 \le \text{board[i].length} \le 200$
* $1 \le \text{word.length} \le 10^3$



## 题目解读

&emsp;在写字板中检索给定的单词是否存在。

```java
class Solution {
    public boolean exist(char[][] board, String word) {

    }
}
```

## 程序设计

* 回溯比较。

```java
class Solution {
    int[] delta = new int[]{1, 0, -1, 0, 1};

    public boolean exist(char[][] board, String word) {
        if (word == null || word.isEmpty()) return false;

        char head = word.charAt(0);
        int m = board.length, n = board[0].length;
        boolean[][] visited = new boolean[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] != head) continue;
                // 回溯
                if (exist(board, visited, word, i, j, 0)) return true;
            }
        }
        return false;
    }

    private boolean exist(char[][] board, boolean[][] visited, String word, int i, int j, int idx) {
        // 匹配成功
        if (idx >= word.length()) return true;
        // 坐标不符合要求或已访问或不相等
        if (i < 0 || j < 0 || i >= board.length || j >= board[0].length || 
            visited[i][j] || board[i][j] != word.charAt(idx)) return false;
        
        // 标记为访问
        visited[i][j] = true;

        for (int k = 0; k < 4; k++) {
            // 试探
            if (exist(board, visited, word, i + delta[k], j + delta[k + 1], idx + 1)) return true;
        }
        // 回溯
        visited[i][j] = false;
        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(4^L)$，空间复杂度为$O(MN)$。

执行用时：7ms，在所有java提交中击败了76.89%的用户。

内存消耗：41.5MB，在所有java提交中击败了14.28%的用户。

## 官方解题

&emsp;暂无，密切关注。