[toc]

Given a non-empty 2D array `grid` of `0`'s and `1`'s, an **island** is a group of `1`'s (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

Find the maximum area of an island in the given 2D array. (If there is no island, the maximum area is 0.)



**Note:** The length of each dimension in the given `grid` does not exceed 50.



## 题目解读

&emsp;查找最大岛屿面积。

```java
class Solution {
    public int maxAreaOfIsland(int[][] grid) {

    }
}
```

## 程序设计

* 矩形的深度优先搜索。

```java
class Solution {
    int[] delta = new int[]{0 , 1, 0, -1, 0};

    public int maxAreaOfIsland(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) return 0;

        int max = 0;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == 0) continue;
                max = Math.max(max, areaOfIsland(grid, i, j));
            }
        }
        return max;
    }

    private int areaOfIsland(int[][] grid, int i, int j) {
        // 非法坐标或非陆地
        if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] == 0) return 0;

        int area = 1;
		// 标记为已遍历
        grid[i][j] = 0;
        for (int k = 0; k < 4; k++) {
            area += areaOfIsland(grid, i + delta[k], j + delta[k + 1]);
        }
        return area;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：3ms，在所有java提交中击败了70.95%的用户。

内存消耗：40MB，在所有java提交中击败了91.67%的用户。

## 官方解题

&emsp;官方还提供了广度优先搜索。