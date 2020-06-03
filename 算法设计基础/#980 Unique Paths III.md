[toc]

On a 2-dimensional `grid`, there are 4 types of squares:

* `1` represents the starting square.  There is exactly one starting square.
* `2` represents the ending square.  There is exactly one ending square.
* `0` represents empty squares we can walk over.
* `-1` represents obstacles that we cannot walk over.

Return the number of 4-directional walks from the starting square to the ending square, that **walk over every non-obstacle square exactly once**.



**Note:**

* $1 \le \text{grid.length} * \text{grid[0].length} \le 20$



## 题目解读

&emsp;求从起始点出发经过每一个非障碍点最终到达终点的路径数目。

```java
class Solution {
    public int uniquePathsIII(int[][] grid) {

    }
}
```

## 程序设计

* 最基本的思路是回溯尝试，当到达终点且所有点已被遍历，则是一条可行路径。

```java
class Solution {
    int count;

    int[] delta = new int[]{0, 1, 0, -1, 0};

    public int uniquePathsIII(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) return 0;

        int m = grid.length, n = grid[0].length;
        // 为0的格子数
        int num = 0;
        // 目标起始坐标
        int sr = 0, sc = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] != -1) num++;
                // 起始点
                if (grid[i][j] == 1) {
                    sr = i;
                    sc = j;
                }
            }
        }

        dfs(grid, sr, sc, num);
        return count;
    }

    private void dfs(int[][] grid, int i, int j, int num) {
        num--;
        // 到达终点
        if (grid[i][j] == 2) {
            // 遍历完成，计数加一
            if (num == 0) count++;
            return;
        }
        // 未到达终点，已遍历完成
        if (num <= 0) return;

        // 标记为已遍历
        grid[i][j] = -2;
        for (int k = 0; k < 4; k++) {
            int x = i + delta[k], y = j + delta[k + 1];
            // 坐标非法或是障碍物或已遍历
            if (x < 0 || x >= grid.length || y < 0 || y >= grid[0].length
                    || grid[x][y] == -1 || grid[x][y] == -2) continue;
            dfs(grid, x, y, num);
        }
        // 遍历结束，恢复
        grid[i][j] = 0;
    }
}
```

## 性能分析

&emsp;时间复杂度上界为$O(4^{MN})$，空间复杂度为$O(MN)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上，官方除了回溯，还提供了记忆化回溯的思路。上面回溯中每个点有三个状态`i`，`j`，`num`，只是此处`num`不能表示剩余的未遍历数，而是表示未遍历的编码路径。由于$1 \le \text{grid.length} * \text{grid[0].length} \le 20$，可以使用数字的二进制来表示是否遍历。

```java
class Solution {
    Integer[][][] record;

    int[] delta = new int[]{0, 1, 0, -1, 0};

    public int uniquePathsIII(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) return 0;

        int m = grid.length, n = grid[0].length;
        // 未遍历路径（未遍历的二进制位为1）
        int path = 0;
        // 目标起始坐标
        int sr = 0, sc = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] != -1) path |= 1 << (i * n + j);
                // 起始点
                if (grid[i][j] == 1) {
                    sr = i;
                    sc = j;
                }
            }
        }

        this.record = new Integer[m][n][1 << m * n];

        // 访问起始点，在路径中置为0
        path ^= 1 << (sr * n + sc);
        return dfs(grid, sr, sc, path);
    }

    private int dfs(int[][] grid, int i, int j, int path) {
        if (record[i][j][path] != null) return record[i][j][path];

        int m = grid.length, n = grid[0].length;
        // 到达终点
        if (grid[i][j] == 2) return path == 0 ? 1 : 0;

        int count = 0;
        for (int k = 0; k < 4; k++) {
            int x = i + delta[k], y = j + delta[k + 1];
            // 坐标非法或是障碍物或已遍历（此时未访问路径中该位为0，相与得到0）
            if (x < 0 || x >= m || y < 0 || y >= n
                    || (path & 1 << (x * n + y)) == 0) continue;
            // 访问x,y，在路径中置为0
            count += dfs(grid, x, y, path ^ (1 << (x * n + y)));
        }
        record[i][j][path] = count;
        return count;
    }
}
```

&emsp;时间复杂度为$O(MN2^{MN})$，空间复杂度为$O(MN2^{MN})$。

执行用时：128ms，在所有java提交中击败了6.62%的用户。

内存消耗：222.4MB，在所有java提交中击败了100.00%的用户。