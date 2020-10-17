[toc]

A virus is spreading rapidly, and your task is to quarantine the infected area by installing walls.

The world is modeled as a 2-D array of cells, where $0$ represents uninfected cells, and $1$ represents cells contaminated with the virus. A wall (and only one wall) can be installed **between any two 4-directionally adjacent cells**, on the shared boundary.

Every night, the virus spreads to all neighboring cells in all four directions unless blocked by a wall. Resources are limited. Each day, you can install walls around only one region -- the affected area (continuous block of infected cells) that threatens the most uninfected cells the following night. There will never be a tie.

Can you save the day? If so, what is the number of walls required? If not, and the world becomes fully infected, return the number of walls used.



**Note**:

* The number of rows and columns of `grid` will each be in the range $[1, 50]$.
* Each `grid[i][j]` will be either $0$ or $1$.
* Throughout the described process, there is always a contiguous viral region that will infect **strictly more** uncontaminated squares in the next round.



## 题目解读

&emsp;给定病毒取余，每天只能隔离一片区域，若最后有未感染区域，则求隔离所用的墙的数目，否则返回已安装的隔离墙。

```java
class Solution {
    public int containVirus(int[][] grid) {

    }
}
```

## 程序设计

* 观察示例发现每次都将感染最多的区域建墙隔离，这样将原问题分为两部分，即计算每个病毒区域的不建墙感染数目和建墙数目，然后选择如果不建墙感染最多的区域建墙；标记模拟传染，将未建墙的区域扩展传染，将建墙的区域标记为$-1$。

```java
class Solution {
    int[] delta = new int[]{1, 0, -1, 0, 1};
    int n, m;

    public int containVirus(int[][] grid) {
        n = grid.length;
        m = grid[0].length;

        int res = 0;
        while (true) {
            boolean complete = true;
            // 计算最多传染的格子和所需的墙
            int mark = 2;
            boolean[][] visited = new boolean[n][m];
            // 记录不建墙传染最多的格子数
            int maxSpred = 0, wall = -1, x = -1, y = -1;
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < m; j++) {
                    if (grid[i][j] != 1 || visited[i][j]) continue;
                    int[] tmp = spreadAndWall(i, j, grid, visited, mark++);
                    if (tmp[0] > maxSpred) {
                        maxSpred = tmp[0];
                        wall = tmp[1];
                        x = i;
                        y = j;
                    }
                }
            }
            // 结束，表示无病毒或病毒都在墙里
            if (x == -1) return res;
            res += wall;

            // 标记新传染的格子
            visited = new boolean[n][m];
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < m; j++) {
                    if (grid[i][j] <= 0 || visited[i][j]) continue;
                    // 将上述标记的格子置为0（主要还原建墙区域的周围格子）
                    if (grid[i][j] > 1) grid[i][j] = 0;
                    // 存在新感染的格子，则不结束
                    else if (mark(i, j, grid, visited, i == x && j == y)) complete = false;
                }
            }

            // 结束，病毒无法继续感染
            if (complete) break;
        }
        return res;
    }

    // 标记周围新传染格子并计算所需墙
    private int[] spreadAndWall(int x, int y, int[][] grid, boolean[][] visited, int mark) {
        int spread = 0, wall = 0;
        visited[x][y] = true;
        for (int i = 0; i < 4; i++) {
            int newX = x + delta[i], newY = y + delta[i + 1];
            if (boundary(newX, newY) || visited[newX][newY]) continue;
            // 感染区域深度优先搜索
            if (grid[newX][newY] == 1) {
                int[] tmp = spreadAndWall(newX, newY, grid, visited, mark);
                spread += tmp[0];
                wall += tmp[1];
            }
            // 非感染区域
            else if (grid[newX][newY] != -1) {
                wall++;
                // 如果不是mark标记的值则感染（避免多统计感染的格子）
                if (grid[newX][newY] != mark) {
                    grid[newX][newY] = mark;
                    spread++;
                }
            }
        }
        return new int[]{spread, wall};
    }

    // 感染外围，标记建墙区域（如果存在新感染格子则返回true）
    private boolean mark(int x, int y, int[][] grid, boolean[][] visited, boolean flag) {
        boolean res = false;
        visited[x][y] = true;
        if (flag) grid[x][y] = -1;
        for (int i = 0; i < 4; i++) {
            int newX = x + delta[i], newY = y + delta[i + 1];
            if (boundary(newX, newY) || visited[newX][newY]) continue;
            // 感染区域深度遍历
            if (grid[newX][newY] == 1) {
                if (mark(newX, newY, grid, visited, flag)) res = true;
            }
            // 非感染区域
            if (!flag && grid[newX][newY] != -1) {
                grid[newX][newY] = 1;
                // 标记为已访问，避免再次扩展
                visited[newX][newY] = true;
                res = true;
            }
        }
        return res;
    }

    private boolean boundary(int x, int y) {
        return x < 0 || x >= n || y < 0 || y >= m;
    }
}
```

## 性能分析

执行用时：5ms，在所有java提交中击败了95.65%的用户。

内存消耗：38.2MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方思路类似。