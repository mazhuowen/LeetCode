[toc]

On a $2 \times 3$ board, there are 5 tiles represented by the integers 1 through 5, and an empty square represented by 0.

A move consists of choosing 0 and a 4-directionally adjacent number and swapping it.

The state of the board is solved if and only if the board is `[[1,2,3],[4,5,0]]`.

Given a puzzle board, return the least number of moves required so that the state of the board is solved. If it is impossible for the state of the board to be solved, return -1.



Note:

* board will be a $2 \times 3$ array as described above.
* `board[i][j]` will be a permutation of `[0, 1, 2, 3, 4, 5]`.



## 题目解读

&emsp;滑动板滑动到所需格式的最少步数，不存在返回-1。

```java
class Solution {
    public int slidingPuzzle(int[][] board) {

    }
}
```

## 程序设计

* 参考官方思路，采用广度优先搜索进行暴力尝试。问题的关键在于写字板的状态记录表示，采用类封装表示。

```java
class Solution {
    int[] delta = new int[]{0, 1, 0, -1, 0};

    public int slidingPuzzle(int[][] board) {
        if (board == null || board.length != 2 || board[0].length != 3) throw new IllegalArgumentException("invalid param");

        // 需要达到的目标值
        String target = Arrays.deepToString(new int[][]{{1, 2, 3}, {4, 5, 0}});
        // 广度优先搜索
        Set<String> set = new HashSet<>();
        Queue<Board> queue = new LinkedList<>();
        // 加入起始位置
        int[] idx = findStart(board);
        Board start = new Board(board, idx[0], idx[1]);
        set.add(start.toString());
        queue.add(start);

        int level = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                Board cur = queue.poll();
                // 到达
                if (target.equals(cur.toString())) return level;

                // 试探移动0
                for (int j = 0; j < 4; j++) {
                    int x = cur.i + delta[j], y = cur.j + delta[j + 1];
                    if (x < 0 || x >= 2 || y < 0 || y >= 3) continue;

                    // 生成滑动板
                    int[][] temp = deepClone(cur.board);
                    temp[cur.i][cur.j] = temp[x][y];
                    temp[x][y] = 0;
                    Board next = new Board(temp, x, y);
                    // 已存在
                    if (set.contains(next.toString())) continue;

                    // 加入队列
                    set.add(next.toString());
                    queue.add(next);
                }
            }
            level++;
        }
        // 不可达
        return -1;
    }

    // 查找起始位置
    private int[] findStart(int[][] board) {
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                if (board[i][j] == 0) return new int[]{i, j};
            }
        }
        throw new IllegalArgumentException("invalid param");
    }

    // 深度拷贝
    private int[][] deepClone(int[][] board) {
        int[][] copy = new int[board.length][board[0].length];
        for (int i = 0; i < board.length; i++) {
            copy[i] = board[i].clone();
        }
        return copy;
    }
}

class Board {
    // 滑动板
    int[][] board;
    // 0的位置
    int i;
    int j;

    public Board(int[][] board, int i, int j) {
        this.board = board;
        this.i = i;
        this.j = j;
    }

    @Override
    public String toString() {
        // 由于数组元素是int[]，不是基本类型，故使用deepToString
        return Arrays.deepToString(this.board);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN(MN)!)$，最多有$(MN)!$中棋盘布局，每个布局最多移动$MN$次；空间复杂度为$O(MN(MN)!)$。

执行用时：35ms，在所有java提交中击败了15.16%的用户。

内存消耗：39.5MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;上述思路参考官方。