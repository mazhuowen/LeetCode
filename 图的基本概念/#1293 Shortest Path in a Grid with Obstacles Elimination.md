[toc]

Given a $m \times n$ grid, where each cell is either `0` (empty) or `1` (obstacle). In one step, you can move up, down, left or right from and to an empty cell.

Return the minimum number of steps to walk from the upper left corner `(0, 0)` to the lower right corner `(m-1, n-1)` given that you can eliminate **at most** `k` obstacles. If it is not possible to find such walk return -1.



**Constraints**:

* $\text{grid.length} == m$
* $\text{grid[0].length} == n$
* $1 \le m, n \le 40$
* $1 \le k \le m \times n$
* `grid[i][j] == 0` or `1`
* `grid[0][0] == grid[m-1][n-1] == 0`



## 题目解读

&emsp;在最多可移除$k$个障碍物的情况下，求到达终点的最短路径。

```java
class Solution {
    public int shortestPath(int[][] grid, int k) {

    }
}
```

## 程序设计

* 最基本的思路是广度优先搜索，入队当前点的坐标和剩余移除次数，将遍历过的点标记避免重复遍历；但该思路有问题，在同层遍历中如果遍历相同点，先入队`k`较小的，后面遍历`k`较大的情况则无法入队，导致结果不准确；其次不仅是同层，不同层同一节点`k`值不同，理应选择最大的`k`。
* 由于是层次遍历无法对不同层操作，引入访问标识三维数组。

```java
class Solution {
    int[] delta = new int[]{0, 1, 0, -1, 0};

    public int shortestPath(int[][] grid, int k) {
        if (grid == null || grid.length == 0) return -1;

        int m = grid.length, n = grid[0].length;

        int level = 0;
        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{0, 0, k});

        // 是否已访问，注意由于和状态k有关，是三维数组
        boolean[][][] visited = new boolean[m][n][k + 1];
        visited[0][0][k] = true;

        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int[] cur = queue.poll();
                // 到达终点
                if (cur[0] == m - 1 && cur[1] == n - 1) return level;

                for (int j = 0; j < 4; j++) {
                    int x = cur[0] + delta[j], y = cur[1] + delta[j + 1];
                    // 坐标不合法或无可移除障碍次数
                    if (x < 0 || x >= m || y < 0 || y >= n || 
                        (cur[2] == 0 && grid[x][y] == 1)) continue;
                    
                    int newK = grid[x][y] == 1 ? cur[2] - 1 : cur[2];
                    // 已遍历，无需再次入队
                    if (visited[x][y][newK]) continue;
                    queue.add(new int[]{x, y, newK});
                    visited[x][y][newK] = true;
                }
            }
            level++;
        }
        return -1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MNK)$，空间复杂度为$O(MNK)$。

执行用时：32ms，在所有java提交中击败了38.59%的用户。

内存消耗：40.6MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;思路同上。参考社区时间性能较好方法，当$K >= m + n - 3$时，最短路径为$m + n - 2$；其次遍历标识数组只需记录最大的`k`，剪枝优化后如下：

```java
class Solution {
    int[] delta = new int[]{0, 1, 0, -1, 0};

    public int shortestPath(int[][] grid, int k) {
        if (grid == null || grid.length == 0) return -1;

        int m = grid.length, n = grid[0].length;

        // 如果可移除的次数大于m + n - 3，则只需从起点出发向下或向右移动，直到终点
        // 遇到障碍物就移除，路径最短为m + n - 2
        if (k >= m + n - 3) return m + n - 2;

        int level = 0;
        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{0, 0, k});

        // 是否已访问，存放到达该位置最大的k
        int[][] visited = new int[m][n];
        for (int[] array : visited) Arrays.fill(array, -1);
        visited[0][0] = k;

        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int[] cur = queue.poll();
                if (cur[0] == m - 1 && cur[1] == n - 1) return level;

                for (int j = 0; j < 4; j++) {
                    int x = cur[0] + delta[j], y = cur[1] + delta[j + 1];
                    if (x < 0 || x >= m || y < 0 || y >= n || 
                        (cur[2] == 0 && grid[x][y] == 1)) continue;
                    
                    int newK = grid[x][y] == 1 ? cur[2] - 1 : cur[2];
                    if (newK <= visited[x][y]) continue;
                    queue.add(new int[]{x, y, newK});
                    visited[x][y] = newK;
                }
            }
            level++;
        }
        return -1;
    }
}
```

执行用时：3ms，在所有java提交中击败了70.78%的用户。

内存消耗：39.1MB，在所有java提交中击败了100.00%的用户。