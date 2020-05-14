[toc]

In a given grid, each cell can have one of three values:

* the value `0` representing an empty cell;
* the value `1` representing a fresh orange;
* the value `2` representing a rotten orange.

Every minute, any fresh orange that is adjacent (4-directionally) to a rotten orange becomes rotten.

Return the minimum number of minutes that must elapse until no cell has a fresh orange.  If this is impossible, return -1 instead.



Note:

* $1 \le \text{grid.length} \le 10$
* $1 \le \text{grid[0].length} \le 10$
* `grid[i][j]` is only `0`, `1`, or `2`.



## 题目解读

&emsp;橘子腐烂的最长时间。

```java
class Solution {
    public int orangesRotting(int[][] grid) {
        
    }
}
```

## 程序设计

* 广度优先搜索。

```java
class Solution {
    int[] delta = new int[]{0, 1, 0, -1, 0};

    public int orangesRotting(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) return 0;
        int m = grid.length, n = grid[0].length;

        // 将腐烂的橘子加入队列
        Queue<int[]> queue = new LinkedList<>();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 2) queue.add(new int[]{i, j});
            }
        }

        // 广度优先遍历，记录层数
        int time = -1;
        while (!queue.isEmpty()) {
            time++;
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int[] cur = queue.poll();
                for (int j = 0; j < 4; j++) {
                    int x = cur[0] + delta[j], y = cur[1] + delta[j + 1];
                    if (x < 0 || x >= m || y < 0 || y >= n || grid[x][y] != 1) continue;

                    grid[x][y] = 2;
                    queue.add(new int[]{x, y});
                }
            }
        }

        // 校验是否还有新鲜橘子
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) return -1;
            }
        }
        return time == -1 ? 0 : time;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：2ms，在所有java提交中击败了98.23%的用户。

内存消耗：39.5MB，在所有java提交中击败了33.33%的用户。

## 官方解题

&emsp;同上。