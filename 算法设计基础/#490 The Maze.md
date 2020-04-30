[toc]

There is a **ball** in a maze with empty spaces and walls. The ball can go through empty spaces by rolling **up**, **down**, **left** or **right**, but it won't stop rolling until hitting a wall. When the ball stops, it could choose the next direction.

Given the ball's **start position**, the **destination** and the **maze**, determine whether the ball could stop at the destination.

The maze is represented by a binary 2D array. 1 means the wall and 0 means the empty space. You may assume that the borders of the maze are all walls. The start and destination coordinates are represented by row and column indexes.



Note:

* There is only one ball and one destination in the maze.
* Both the ball and the destination exist on an empty space, and they will not be at the same position initially.
* The given maze does not contain border (like the red rectangle in the example pictures), but you could assume the border of the maze are all walls.
* The maze contains at least 2 empty spaces, and both the width and height of the maze won't exceed 100.



## 题目解读

&emsp;迷宫中球只有碰到墙才会停止并选择下一个滚动方向，判断从起点是否可到达终点。

```java
class Solution {
    public boolean hasPath(int[][] maze, int[] start, int[] destination) {

    }
}
```

## 程序设计

* 每个空位置有两个状态，即当前运动方向没有墙壁，则继续在该方向滚动；当前方向后一个位置是墙壁，则需要判断分析：如果当前点是目标点，则可到达；如果当前点已经遍历过，则说明存在循环，到达不了（对于路径拐点必须只能遍历一次）；选择新的方向和起始点继续试探。
* 需注意，路径点除了拐点其它点可以被复用。核心思路就是拐点不能访问第二次。

```java
class Solution {
    int[] delta = new int[]{1, 0, -1, 0 , 1};
    int endX, endY;

    public boolean hasPath(int[][] maze, int[] start, int[] destination) {
        if (maze == null || maze.length == 0 || maze[0].length == 0) return false;
        endX = destination[0];
        endY = destination[1];
        // 从起始点尝试四个方向
        for (int i = 0; i < 4; i++) {
            if (hasPath(maze, start[0], start[1], i)) return true;
        }
        return false;
    }

    private boolean hasPath(int[][] maze, int i, int j, int direction) {
        int m = maze.length, n = maze[0].length;
        // 不是有效空位置
        if (i < 0 || i >= m || j < 0 || j >= n || maze[i][j] == 1) return false;

        // 表示球的路径（除了墙壁处的拐点，其它路径点都可以复用，此处减一，与墙壁1区分）
        maze[i][j]--;

        // 该方向下一个位置
        int nextI = i + delta[direction];
        int nextJ = j + delta[direction + 1];

        // 下一个位置是墙壁，即当前点是拐点，选择新的起点和方向
        if (nextI < 0 || nextI >= m || nextJ < 0 || nextJ >= n || maze[nextI][nextJ] == 1) {
            // 到达终点，选择停止
            if (i == endX && j == endY) return true;
            // 拐点已经遍历过，再次遍历说明循环
            if (maze[i][j] != -1) return false;
            // 尝试四个方向
            for (int d = 0; d < 4; d++) {
                nextI = i + delta[d];
                nextJ = j + delta[d + 1];
                if (hasPath(maze, nextI, nextJ, d)) return true; 
            }
        } 
        // 没有碰到墙壁，继续在该方向滚动
        else {
            if (hasPath(maze, nextI, nextJ, direction)) return true;
        }
        // 回溯
        maze[i][j]++;
        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：2ms，在所有java提交中击败了95.56%的用户。

内存消耗：41.5MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方解题采用深度优先搜索和广度优先搜索的思路。

```java
public class Solution {
    public boolean hasPath(int[][] maze, int[] start, int[] destination) {
        // 标识拐点是否被访问
        boolean[][] visited = new boolean[maze.length][maze[0].length];
        return dfs(maze, start, destination, visited);
    }
    public boolean dfs(int[][] maze, int[] start, int[] destination, boolean[][] visited) {
        // 拐点被访问，即循环
        if (visited[start[0]][start[1]]) return false;
        // 找到目标点
        if (start[0] == destination[0] && start[1] == destination[1]) return true;
        
        // 标记拐点为访问
        visited[start[0]][start[1]] = true;
        // 上下左右坐标
        int r = start[1] + 1, l = start[1] - 1, u = start[0] - 1, d = start[0] + 1;
        // 查找向右方向的拐点
        while (r < maze[0].length && maze[start[0]][r] == 0) r++;
        if (dfs(maze, new int[] {start[0], r - 1}, destination, visited)) return true;
        
        // 查找向左方向的拐点
        while (l >= 0 && maze[start[0]][l] == 0) l--;
        if (dfs(maze, new int[] {start[0], l + 1}, destination, visited)) return true;
        
        // 查找向上方向的拐点
        while (u >= 0 && maze[u][start[1]] == 0) u--;
        if (dfs(maze, new int[] {u + 1, start[1]}, destination, visited)) return true;
        
        // 查找向下方向的拐点
        while (d < maze.length && maze[d][start[1]] == 0) d++;
        if (dfs(maze, new int[] {d - 1, start[1]}, destination, visited)) return true;
        
        // 没查到
        return false;
    }
}
```

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：2ms，在所有java提交中击败了95.56%的用户。

内存消耗：41.5MB，在所有java提交中击败了100.00%的用户。

```java
public class Solution {
    public boolean hasPath(int[][] maze, int[] start, int[] destination) {
        // 标记拐点
        boolean[][] visited = new boolean[maze.length][maze[0].length];
        int[][] dirs={{0, 1}, {0, -1}, {-1, 0}, {1, 0}};
        Queue < int[] > queue = new LinkedList < > ();
        queue.add(start);
        visited[start[0]][start[1]] = true;
        while (!queue.isEmpty()) {
            int[] s = queue.poll();
            // 找到
            if (s[0] == destination[0] && s[1] == destination[1]) return true;
            // 尝试四个方向
            for (int[] dir: dirs) {
                int x = s[0] + dir[0];
                int y = s[1] + dir[1];
                // 查找拐点
                while (x >= 0 && y >= 0 && x < maze.length && y < maze[0].length && maze[x][y] == 0) {
                    x += dir[0];
                    y += dir[1];
                }
                // 加入队列
                if (!visited[x - dir[0]][y - dir[1]]) {
                    queue.add(new int[] {x - dir[0], y - dir[1]});
                    visited[x - dir[0]][y - dir[1]] = true;
                }
            }
        }
        return false;
    }
}
```

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：6ms，在所有java提交中击败了41.98%的用户。

内存消耗：40.6MB，在所有java提交中击败了100.00%的用户。