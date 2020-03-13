[toc]

Given a 2d grid map of `'1'`s (land) and `'0'`s (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.



## 题目解读

&emsp;陆地用1表示，水用0表示，计算岛屿的数量。

```java
class Solution {
    public int numIslands(char[][] grid) {
        
    }
}
```

## 程序设计

* 首先定义不相交集，由于需要知道岛屿即不相交集的数目，在不相交集类中定义数量记录。

```java
class DisJoint {
    private int size;
    private int[] tree;

    DisJoint(int size) {
        this.size = size;
        this.tree = new int[size];
        // 初始化为size个不相交集
        for(int i = 0; i < size; i++) {
            tree[i] = -1;
        }
    }
    // 按规模并
    public void union(int root1, int root2) {
        if(tree[root1] >= 0 || tree[root2] >= 0) {
            throw new IllegalArgumentException("not a root");
        }
        if(root1 == root2) {
            return;
        }
        // 树2大，树1并入树2
        if(tree[root1] >= tree[root2]) {
            tree[root2] += tree[root1];
            tree[root1] = root2;
        } else {
            tree[root1] += tree[root2];
            tree[root2] = root1;
        }
        // 合并后集合数目减少
        size--;
    }
    // 路径压缩
    public int find(int val) {
        if(tree[val] < 0) return val;
        return tree[val] = find(tree[val]);
    }
    // 返回集合数目
    public int getSize() {
        return size;
    }
}
```

* 遍历矩形，将陆地和邻接陆地合并，同时记录水的数量，最后答案就是不相交集数量减去水的数量。

```java
class Solution {
    public int numIslands(char[][] grid) {
        if(grid == null || grid.length == 0) {
            return 0;
        }
        int m = grid.length, n = grid[0].length;
        int waterCount = 0;
        DisJoint disJoint = new DisJoint(m * n);
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                char c = grid[i][j];
                int idx = i * n + j;
                if(c == '1') {
                    // 加入前一个岛屿
                    if(j - 1 >= 0 && grid[i][j - 1] == '1') {
                        disJoint.union(disJoint.find(idx - 1), disJoint.find(idx));
                    }
                    // 加入上一个岛屿
                    if(i - 1 >= 0 && grid[i - 1][j] == '1') {
                        disJoint.union(disJoint.find(idx - n), disJoint.find(idx));
                    }
                } else {
                    waterCount++;
                }
            }
        }
        return disJoint.getSize() - waterCount;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(NM)$。

执行用时：6ms，在所有java提交中击败了26.44%的用户。

内存消耗：42.1MB，在所有java提交中击败了5.02%的用户。

## 官方解题

&emsp;除了不相交集，官方还提供了深度遍历和广度遍历。

```java
class Solution {
    public int numIslands(char[][] grid) {
        if(grid == null || grid.length == 0) {
            return 0;
        }
        int m = grid.length, n = grid[0].length;
        boolean[][] visit = new boolean[m][n];
        int count = 0;
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                // 未访问，计数加一
                if(grid[i][j] == '1' && !visit[i][j]) {
                    count++;
                    // 深度优先遍历
                    dfs(grid, visit, i, j);
                }
            }
        }
        return count;
    }

    private void dfs(char[][] grid, boolean[][] visit, int row, int col) {
        if(row < 0 || row >= grid.length || col < 0 || col >= grid[0].length) {
            return;
        }
        if(grid[row][col] == '0' || visit[row][col]) {
            return;
        }
        visit[row][col] = true;
        dfs(grid, visit, row - 1, col);
        dfs(grid, visit, row + 1, col);
        dfs(grid, visit, row, col - 1);
        dfs(grid, visit, row, col + 1);
    }
}
```

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(NM)$。

执行用时：2ms，在所有java提交中击败了93.74%的用户。

内存消耗：41.7MB，在所有java提交中击败了6.04%的用户。

> 官方提供思路不使用额外的数组记录是否访问过，而是将访问过的结点置为0。