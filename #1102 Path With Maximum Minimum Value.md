[toc]

Given a matrix of integers `A` with R rows and C columns, find the **maximum** score of a path starting at `[0,0]` and ending at `[R-1,C-1]`.

The score of a path is the **minimum** value in that path.  For example, the value of the path $8 \to  4 \to  5 \to  9$ is $4$.

A path moves some number of times from one visited cell to any neighbouring unvisited cell in one of the 4 cardinal directions (north, east, west, south).



**Note:**

* $1 \le R, C \le 100$
* $0 \le \text{A[i][j]} \le 10^9$



## 题目解读

&emsp;路径的得分是该路径上的最小值。找出所有路径中得分最高的那条路径，返回其得分。

```java
class Solution {
    public int maximumMinimumPath(int[][] A) {

    }
}
```

## 程序设计

* 最基本的解法是深度优先搜索，穷举遍历，但是会超时。

```java
class Solution {
    public int maximumMinimumPath(int[][] A) {
        if (A == null || A.length == 0) throw new IllegalArgumentException("invalid Param");
        boolean[][] visited = new boolean[A.length][A[0].length];
        return dfs(0, 0, A, visited);

    }

    private int dfs(int x, int y, int[][] A, boolean[][] visited) {
        // 已经遍历过，返回-1
        if (visited[x][y]) return -1;
        // 终点
        if (x == A.length - 1 && y == A[0].length - 1) return A[x][y];

        // 标记为正在访问
        visited[x][y] = true;

        int[] dx = new int[]{-1, 1, 0, 0};
        int[] dy = new int[]{0, 0, -1, 1};
        // 子路径中最大的最小路径
        int maxPath = -1;
        for (int i = 0; i < 4; i++) {
            int newX = x + dx[i], newY = y + dy[i];
            if (newX < 0 || newX >= A.length || newY < 0 || newY >= A[0].length
                    || visited[newX][newY]) continue;

            int childVal = dfs(newX, newY, A, visited);
            if (childVal != -1) {
                maxPath = Math.max(maxPath, childVal);
            }
        }

        visited[x][y] = false;
        return temp == -1 ? -1 : Math.min(maxPath, A[x][y]);
    }
}
```

## 性能分析



## 官方解题