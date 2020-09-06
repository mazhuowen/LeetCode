[toc]

Given a 2D `grid` consisting of `1`s (land) and `0`s (water).  An island is a maximal 4-directionally (horizontal or vertical) connected group of `1`s.

The grid is said to be **connected** if we have **exactly one island**, otherwise is said **disconnected**.

In one day, we are allowed to change **any** single land cell `(1)` into a water cell `(0)`.

Return the minimum number of days to disconnect the grid.



**Constraints:**

- $1 \le \text{grid.length, grid[i].length} \le 30$
- `grid[i][j]` is $0$ or $1$.



## 题目解读

&emsp;给定二维数组表示的岛屿，求最少切分陆地块，使得不再是一块岛屿。

```java
class Solution {
    public int minDays(int[][] grid) {
        
    }
}
```

## 程序设计

* 该题目具有极大误导性，容易理解为图的联通问题，事实上是一个脑筋急转弯题目。首先需要判断初始联通分量，如果没有岛屿或有多个岛屿，则无需切分。
* 考虑只有一块岛屿的情况，由于题目定义连通为垂直于水平方向连接，假设岛屿的右下角位置，其左侧、上方都与其他陆地相连，则只要把这两块陆地去除，右下角的陆地就会和其他陆地断开连接，也就是说最多只需去除两块陆地就可达到题目的要求。
* 从而可以遍历判断去除一个岛屿的情况，如果满足则返回$1$，否则返回$2$。

```java
class Solution {
    private int[] delta = new int[]{0, -1, 0, 1, 0};

    public int minDays(int[][] grid) {
        // 无需切割
        if (!isOneLand(grid)) return 0;

        int n = grid.length, m = grid[0].length;
        // 遍历每块，并尝试切割当前块
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] == 0) continue;
                grid[i][j] = 0;
                // 当前块消除后岛屿变为两个，返回
                if (!isOneLand(grid)) return 1;
                grid[i][j] = 1;
            }
        }
        // 切分任意一个棱角的左边（右边）、上边（下边）两块陆地
        return 2;
    }

    private boolean isOneLand(int[][] grid) {
        int n = grid.length, m = grid[0].length;
        // 位置n * m为所有非陆地集合
        DisJoint disJoint = new DisJoint(n * m + 1);
        for (int r = 0; r < n; r++) {
            for (int c = 0; c < m; c++) {
                int idx = r * m + c;
                // 将水合并
                if (grid[r][c] == 0) disJoint.union(disJoint.find(n * m), disJoint.find(idx));
                // 陆地合并
                else {
                    if (r > 0 && grid[r - 1][c] == 1) disJoint.union(disJoint.find(idx - m), disJoint.find(idx));
                    if (c > 0 && grid[r][c - 1] == 1) disJoint.union(disJoint.find(idx - 1), disJoint.find(idx));
                }
            }
        }

        // 如果只有一个岛，则不相交集只有水和岛两个（只有水和有多个岛则无需切分）
        return disJoint.size() == 2;
    }
}

class DisJoint {
    int size;
    int[] parent;

    DisJoint(int size) {
        this.size = size;
        this.parent = new int[size];
        Arrays.fill(parent, -1);
    }

    public void union(int r1, int r2) {
        if (parent[r1] >= 0 || parent[r2] >= 0) throw new IllegalArgumentException("invalid root");
        if (r1 == r2) return;

        if (parent[r1] <= parent[r2]) {
            parent[r1] += parent[r2];
            parent[r2] = r1;
        } else {
            parent[r2] += parent[r1];
            parent[r1] = r2;
        }
        size--;
    }

    public int find(int idx) {
        if (parent[idx] < 0) return idx;
        return parent[idx] = find(parent[idx]);
    }

    public int size() {
        return this.size;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2M^2)$，空间复杂度为$O(NM)$。

执行用时：25ms，在所有java提交中击败了89.12%的用户。

内存消耗：39.7MB，在所有java提交中击败了39.95%的用户。

## 官方解题

&emsp;暂无，密切关注。

