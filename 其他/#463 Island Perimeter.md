[toc]

You are given a map in form of a two-dimensional integer grid where 1 represents land and 0 represents water.

Grid cells are connected horizontally/vertically (not diagonally). The grid is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells).

The island doesn't have "lakes" (water inside that isn't connected to the water around the island). One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.

<img src="../images/#463.png"  />

## 题目解读

&emsp;计算岛屿周长。

```java
class Solution {
    public int islandPerimeter(int[][] grid) {
        
    }
}
```

## 程序设计

* 仔细观察岛屿周长为构成岛屿的正方形的边减去重叠的边。

```java
class Solution {
    public int islandPerimeter(int[][] grid) {
        // 岛屿方块总数
        int count = 0;
        // 岛屿方块重叠边总数
        int edge = 0;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == 0) continue;
                // 岛屿方块计数
                count++;
                // 重叠边计数
                if (j + 1 < grid[0].length && grid[i][j + 1] == 1) edge++;
                if (i + 1 < grid.length && grid[i + 1][j] == 1) edge++;
            }
        }
        return 4 * count - 2 * edge;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(1)$。

执行用时：7ms，在所有java提交中击败了95.37%的用户。

内存消耗：40.6MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。