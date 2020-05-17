[toc]

On an $N \times N$ board, the numbers from $1$ to $N*N$ are written boustrophedonically **starting from the bottom left of the board**, and alternating direction each row.  For example, for a $6 \times 6$ board, the numbers are written as follows:

<img src="../images/#909.png" style="zoom: 40%;" />

You start on square `1` of the board (which is always in the last row and first column).  Each move, starting from square `x`, consists of the following:

* You choose a destination square `S` with number `x+1`, `x+2`, `x+3`, `x+4`, `x+5`, or `x+6`, provided this number is $\le N*N$.
  * (This choice simulates the result of a standard 6-sided die roll: ie., there are always **at most 6 destinations, regardless of the size of the board**.)
* If `S` has a snake or ladder, you move to the destination of that snake or ladder.  Otherwise, you move to `S`.

A board square on row `r` and column `c` has a "snake or ladder" if `board[r][c] != -1`.  The destination of that snake or ladder is `board[r][c]`.

Note that you only take a snake or ladder at most once per move: if the destination to a snake or ladder is the start of another snake or ladder, you do **not** continue moving.  (For example, if the board is `[[4,-1],[-1,3]]`, and on the first move your destination square is `2`, then you finish your first move at `3`, because you do **not** continue moving to `4`.)

Return the least number of moves required to reach square $N*N$.  If it is not possible, return `-1`.



Note:

* $2 \le \text{board.length} = \text{board[0].length} \le 20$
* `board[i][j]` is between $1$ and $N*N$ or is equal to `-1`.
* The board square with number `1` has no snake or ladder.
* The board square with number $N*N$ has no snake or ladder.



## 题目解读

&emsp;给定蛇梯棋，判断到达终点的最少移动次数。

```java
class Solution {
    public int snakesAndLadders(int[][] board) {

    }
}
```

## 程序设计

* 最基本的思路是回溯，需要注意存在环的情况，但该方法时间复杂度为$O(6^N)$，会超时。

```java
class Solution {
    int  minCount = Integer.MAX_VALUE;
    boolean[] visited;

    public int snakesAndLadders(int[][] board) {
        if (board == null || board.length == 0 || board.length != board[0].length) return 0;

        this.visited = new boolean[board.length * board.length];
        snakesAndLadders(board, 1, 0);
        return minCount == Integer.MAX_VALUE ? -1 : minCount;
    }

    private void snakesAndLadders(int[][] board, int no, int count) {
        int n = board.length;
        // 终止条件
        if (no == n * n) {
            minCount = Math.min(minCount, count);
            return;
        }
        // 已被访问过（解决存在环的问题）或当前次数大于最小值，剪枝
        if (visited[no] || count > minCount) return;

        visited[no] = true;
        
        int newNo = no + 6;
        // 从高到低遍历尝试，配合剪枝减少遍历次数
        for (int i = 0; i < 6; i++, newNo--) {
            if (newNo > n * n) continue;

            int[] cur = transfer(newNo, n);
            int snakeOrLadder = board[cur[0]][cur[1]];
            // 存在蛇或梯子，则新的编号为蛇或梯子的指向
            if (snakeOrLadder != -1) snakesAndLadders(board, snakeOrLadder, count + 1);
            // 不存在，则是新的编号
            else snakesAndLadders(board, newNo, count + 1);
        }
        // 回溯
        visited[no] = false;
    }

    // 将编号转换为坐标
    private int[] transfer(int no, int n) {
        no--;
        int row = no / n;
        int col = no % n;
        return new int[]{n - row - 1, row % 2 == 0 ? col : (n - col - 1)};
    }
}
```

* 事实上可以从起点广度优先搜索。

```java
class Solution {
    public int snakesAndLadders(int[][] board) {
        if (board == null || board.length == 0) return 0;

        int n = board.length, dis = 0;
        // 记录访问点，避免环和重复路径
        boolean[] visited = new boolean[n * n];
        Queue<Integer> queue = new LinkedList<>();
        // 加入起点
        queue.add(1);
        visited[0] = true;
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int curNo = queue.poll();
                // 到达终点
                if (curNo == n * n) return dis;

                // 加入下一步结点
                for (int no = curNo + 6; no > curNo; no--) {
                    if (no > n * n || visited[no - 1]) continue;
                    // 标记为已访问
                    visited[no - 1] = true;
                    int[] cood = transfer(no, n);
                    // 有梯子或蛇
                    int snakeOrLadder = board[cood[0]][cood[1]];
                    if (snakeOrLadder != -1) queue.add(snakeOrLadder);
                    else queue.add(no);
                }
            }
            // 距离加一
            dis++;
        }
        // 无法到达
        return -1;
    }

    // 将编号转换为坐标
    private int[] transfer(int no, int n) {
        no--;
        int row = no / n;
        int col = no % n;
        return new int[]{n - row - 1, row % 2 == 0 ? col : (n - col - 1)};
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：4ms，在所有java提交中击败了97.27%的用户。

内存消耗：39.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方思路同上。