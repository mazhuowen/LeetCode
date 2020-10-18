[toc]

We are given a 2-dimensional `grid`. `"."` is an empty cell, `"#"` is a wall, `"@"` is the starting point, `("a", "b", ...)` are keys, and `("A", "B", ...)` are locks.

We start at the starting point, and one move consists of walking one space in one of the 4 cardinal directions.  We cannot walk outside the grid, or walk into a wall.  If we walk over a key, we pick it up.  We can't walk over a lock unless we have the corresponding key.

For some $1 \le K \le 6$, there is exactly one lowercase and one uppercase letter of the first $K$ letters of the English alphabet in the grid.  This means that there is exactly one key for each lock, and one lock for each key; and also that the letters used to represent the keys and locks were chosen in the same order as the English alphabet.

Return the lowest number of moves to acquire all keys.  If it's impossible, return $-1$.



**Note**:

* $1 \le \text{grid.length} \le 30$
* $1 \le \text{grid[0].length} \le 30$
* `grid[i][j]` contains only `'.'`, `'#'`, `'@'`, `'a'`-`'f'` and `'A'`-`'F'`
* The number of keys is in $[1, 6]$.  Each key has a different letter and opens exactly one lock.



## 题目解读

&emsp;给定二维数组，其中包含墙、锁和对应的钥匙，只有得到相应的钥匙，才能穿过锁，求获取所有钥匙的最少的移动次数。

```java
class Solution {
    public int shortestPathAllKeys(String[] grid) {

    }
}
```

## 程序设计

* 参考社区思路，采用广度优先搜索来确定最短步数，采用二进制状态来标记是否访问过。

```java
class Solution {
    int[] delta = new int[]{1, 0, -1, 0, 1};

    public int shortestPathAllKeys(String[] grid) {
        int n = grid.length, m = grid[0].length();
        char[][] matrix = new char[n][m];
        // 最多6个钥匙，64个状态
        boolean[][][] visited = new boolean[n][m][64];
        Queue<int[]> queue = new LinkedList<>();
        // 钥匙状态
        int finalStat = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                matrix[i][j] = grid[i].charAt(j);
                if (matrix[i][j] == '@') queue.add(new int[]{i, j, 0});
                else if (matrix[i][j] >= 'a' && matrix[i][j] <= 'f') finalStat |= 1 << (matrix[i][j] - 'a');
            }
        }

        int moves = 0;
        // 广度优先搜索
        while (!queue.isEmpty()) {
            moves++;
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int[] cur = queue.poll();
                int x = cur[0], y = cur[1], stat = cur[2];
                for (int j = 0; j < 4; j++) {
                    int newX = x + delta[j], newY = y + delta[j + 1], newStat = stat;
                    // 坐标不合法或是墙或没有对应的钥匙
                    if (newX < 0 || newX >= n || newY < 0 || newY >= m || matrix[newX][newY] == '#'
                        || matrix[newX][newY] >= 'A' && matrix[newX][newY] <= 'F' && (newStat & (1 << (matrix[newX][newY] - 'a'))) == 0) continue;
                    // 如果是钥匙，则加入当前状态
                    if (matrix[newX][newY] >= 'a' && matrix[newX][newY] <= 'f') newStat |= 1 << (matrix[newX][newY] - 'a');
                    // 之前已经访问过
                    if (visited[newX][newY][newStat]) continue;
                    visited[newX][newY][newStat] = true;

                    if (newStat == finalStat) return moves;
                    queue.add(new int[]{newX, newY, newStat});
                    
                }
            }
        }
        return -1;
    }
}
```

## 性能分析

执行用时：15ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.5MB，在所有java提交中击败了92.16%的用户。

## 官方解题

&emsp;官方采用`Dijkstra`计算关键节点的距离。