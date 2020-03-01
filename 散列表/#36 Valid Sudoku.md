[toc]

Determine if a $9 \times 9$ Sudoku board is valid. Only the filled cells need to be validated **according to the following rules**:

* Each row must contain the digits $1\sim9$ without repetition.
* Each column must contain the digits $1\sim9$ without repetition.
* Each of the 9 $3 \times 3$ sub-boxes of the grid must contain the digits $1\sim9$ without repetition.

<img src="../images/#36.png"  />

The Sudoku board could be partially filled, where empty cells are filled with the character `.`.

Note:

* A Sudoku board (partially filled) could be valid but is not necessarily solvable.
* Only the filled cells need to be validated according to the mentioned rules.
* The given board contain only digits $1\sim9$ and the character `.`.
* The given board size is always $9\times9$.



## 题目解读

&emsp;检测数独游戏中已有的排序是否有效。存在数字的位置是数字，还未填充的位置是`.`。

```java
class Solution {
    public boolean isValidSudoku(char[][] board) {

    }
}
```

## 程序设计

* 最简单粗暴的方法是对应每一行、每一列、每一个子空间维护一个集合，遍历的过程遍历是否存在，存在则不符合要求。

```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        // 分别记录9行、9列、9个子单元组合
        Set<Character>[] rows = new Set[9];
        Set<Character>[] cols = new Set[9];
        Set<Character>[] boxs = new Set[9];
        // 初始化
        for(int i = 0; i < 9; i++) {
            rows[i] = new HashSet<>();
            cols[i] = new HashSet<>();
            boxs[i] = new HashSet<>();
        }
        // 遍历二维数组
        for(int row = 0; row < board.length; row++) {
            for(int col = 0; col < board[0].length; col++) {
                // 不是数字，继续
                if(board[row][col] == '.') continue;
                // 已包含，返回无效
                if(rows[row].contains(board[row][col])) return false; 
                rows[row].add(board[row][col]);
                // 已包含，返回无效
                if(cols[col].contains(board[row][col])) return false;
                cols[col].add(board[row][col]);
                // 子格子的编号
                int idx = (row / 3) * 3 + col / 3; 
                // 已包含，返回无效
                if(boxs[idx].contains(board[row][col])) return false;
                boxs[idx].add(board[row][col]);
            }
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：3ms，在所有java提交中击败了68.87%的用户。

内存消耗：41.2MB，在所有java提交中击败了93.85%的用户。

## 官方解题

&emsp;同上。社区一些时间性能较快的方法使用了类似位图的思路，不使用集合类还是通过一个整数来表示每行、每列、每个子空间包含的数字，通过位操作来判断集合关系。