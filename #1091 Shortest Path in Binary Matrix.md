[toc]

In an N by N square grid, each cell is either empty (0) or blocked (1).

A clear path from top-left to bottom-right has length `k` if and only if it is composed of cells $C_1, C_2, \cdots, C_k$ such that:

* Adjacent cells $C_i$ and $C_{i+1}$ are connected 8-directionally (ie., they are different and share an edge or corner)
* $C_1$ is at location `(0, 0)` (ie. has value `grid[0][0]`)
* $C_k$ is at location `(N-1, N-1)` (ie. has value `grid[N-1][N-1]`)
* If $C_i$ is located at `(r, c)`, then `grid[r][c]` is empty (ie. `grid[r][c] == 0`).

Return the length of the shortest such clear path from top-left to bottom-right.  If such a path does not exist, return -1.



**Note**:

* $1 \le \text{grid.length} == \text{grid[0].length} \le 100$
* `grid[r][c]` is `0` or `1`



## 题目解读

&emsp;

```java
class Solution {
    public int shortestPathBinaryMatrix(int[][] grid) {

    }
}
```

## 程序设计

* 

```java
class Solution {
    int[] delta = new int[]{0, 1, 1, -1, -1, 1, 0, -1, 0};

    public int shortestPathBinaryMatrix(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) return -1;

        int m = grid.length, n = grid[0].length;
        if (grid[0][0] != 0 || grid[m - 1][n - 1] != 0) return -1;
        boolean[] visited = new boolean[m * n];

        Queue<Integer> queue = new LinkedList<>();

        int level = 0;
        queue.add(0);
        visited[0] = true;
        while (!queue.isEmpty()) {
            level++;
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int cur = queue.poll();
                if (cur == m * n - 1) return level;

                int x = cur / n, y = cur % n;
                for (int j = 0; j < 8; j++) {
                    int newX = x + delta[j], newY = y + delta[j + 1];
                    if (newX < 0 || newX >= m || newY < 0 || newY >= n 
                        || grid[newX][newY] != 0 || visited[newX * n + newY]) continue;
                    visited[newX * n + newY] = true;
                    queue.add(newX * n + newY);
                }
            }
        }
        return -1;
    }
}
```

## 性能分析

&emsp;

执行用时：19 ms, 在所有 Java 提交中击败了60.76%的用户

内存消耗：41 MB, 在所有 Java 提交中击败了100.00%的用户

## 官方解题

&emsp;