[toc]

Given a non-empty 2D array `grid` of `0`'s and `1`'s, an **island** is a group of `1`'s (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

Count the number of **distinct** islands. An island is considered to be the same as another if and only if one island can be translated (and not rotated or reflected) to equal the other.



**Note:** The length of each dimension in the given `grid` does not exceed 50.



## 题目解读

&emsp;找出不同的岛屿数量。此处不同的岛屿形状指不可通过平移得到，不考虑翻转、旋转。

```java
class Solution {
    public int numDistinctIslands(int[][] grid) {

    }
}
```

## 程序设计

* 岛屿可通过深度优先搜索遍历，问题在于怎么判断岛屿形状。初步想到可以使用深度优先搜索遍历的路径来表示一个岛屿，但正如前序序列或后序序列不能唯一决定一棵二叉树，岛屿的路径也不能唯一决定，如$_1^111$和$11 _1^1$的路径一样。
* 为了能够区分，在路径中加入`#`，每次深度搜索都是用`#`来标识。

```java
class Solution {
    int[] delta = new int[]{0, 1, 0, -1, 0};
    Set<String> set;

    public int numDistinctIslands(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) return 0;

        this.set = new HashSet<>();
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == 0) continue;

                // -1表示起始
                StringBuffer path = new StringBuffer("-1");
                numDistinctIslands(grid, i, j, path);
                set.add(path.toString());
            }
        }
        return set.size();
    }

    private void numDistinctIslands(int[][] grid, int i, int j, StringBuffer path) {
        int m = grid.length, n = grid[0].length;

        grid[i][j] = 0;
        for (int k = 0; k < 4; k++) {
            path.append("#");
            // 试探四个方向，在路径中加入方向索引
            int x = i + delta[k], y = j + delta[k + 1];
            if (x < 0 || x >= m || y < 0 || y >= n || grid[x][y] == 0) continue;

            path.append(k);
            numDistinctIslands(grid, x, y, path);
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：11ms，在所有java提交中击败了50.68%的用户。

内存消耗：40.3MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;思路同上。