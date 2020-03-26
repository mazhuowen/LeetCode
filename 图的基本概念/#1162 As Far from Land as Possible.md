[toc]

Given an $N \times N$ `grid` containing only values `0` and `1`, where `0` represents water and `1` represents land, find a water cell such that its distance to the nearest land cell is maximized and return the distance.

The distance used in this problem is the Manhattan distance: the distance between two cells $(x_0, y_0)$ and $(x_1, y_1)$ is $\vert x_0 - x_1\vert + \vert y_0 - y_1\vert$.

If no land or water exists in the grid, return `-1`.



**Note:**

* $1 \le \text{grid.length} == \text{grid[0].length} \le 100$
* $\text{grid[i][j]}$ is `0` or `1`



## 题目解读

&emsp;给定矩阵，`1`代表陆地，`0`代表水，找到离陆地最远的水的区域到陆地的距离。

```java
class Solution {
    public int maxDistance(int[][] grid) {

    }
}
```

## 程序设计

* 首先遍历找到所有的陆地，然后层次遍历，直到最后剩下的就是离陆地最远的区域，而层次遍历的层数则是距离。
* 需要注意每次遍历入队，需要在入队的时候将入队区域标记为1。

```java
class Solution {
    public int maxDistance(int[][] grid) {
        if (grid == null || grid.length == 0) return -1;

        int n = grid.length;
        // 将陆地入队
        Queue<int[]> queue = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) queue.add(new int[]{i, j});
            }
        }
        // 只有陆地或海洋
        if (queue.isEmpty() || queue.size() == n * n) return -1;

        int[] dx = new int[]{-1, 1, 0 ,0};
        int[] dy = new int[]{0, 0, -1 ,1};
        // 广度优先搜索
        int count = 0;
        while (!queue.isEmpty()) {
            // 同层出队
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int[] cur = queue.poll();
                for (int j = 0; j < 4; j++) {
                    int x = cur[0] + dx[j], y = cur[1] + dy[j];
                    if (x < 0 || x >= n || y < 0 || y >= n || grid[x][y] == 1) continue;
                    // 将入队的区域标记为1
                    grid[x][y] = 1;
                    queue.add(new int[]{x, y});
                }
            }
            count++;
        }
        return count - 1;
    }
}
```

## 性能分析

&emsp;时间复杂度为 $O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：15ms，在所有java提交中击败了92.79%的用户。

内存消耗：42.2MB，在所有java提交中击败了97.39%的用户。

## 官方解题

&emsp;官方提供了好几种复杂的思路，繁杂又不具通用性，不做了解。