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

* 基本思路为采用深度优先搜索遍历所有路径。需注意到本题的特殊性，若一般的，将球滚动的路径标记为已访问，则墙壁反弹后路径交叉的情况会无法重现，如果不标记路径，则存在两个墙壁间反复反弹的情况；仔细观察发现拐点不能重复，需要对拐点进行标记。

```java
class Solution {
    int[] delta = new int[]{0, 1, 0, -1, 0};

    public boolean hasPath(int[][] maze, int[] start, int[] destination) {
        if (maze == null || maze.length == 0 || maze[0].length == 0) throw new IllegalArgumentException("invalid param");

        // 拐点访问标识
        boolean[][] visited = new boolean[maze.length][maze[0].length];
        return hasPath(maze, visited, start, destination);
    }

    private boolean hasPath(int[][] maze, boolean[][] visited, int[] cur, int[] destination) {
        int x = cur[0], y = cur[1];
        int n = maze.length, m = maze[0].length;
        // 到达终点
        if (x == destination[0] && y == destination[1]) return true;

        // 拐点已遍历
        if (visited[x][y]) return false;
        visited[x][y] = true;

        // 从当前拐点出发到达下一个拐点
        for (int i = 0; i < 4; i++) {
            int newX = x, newY = y;
            while (newX >= 0 && newX < n && newY >= 0 && newY < m && maze[newX][newY] == 0) {
                newX += delta[i];
                newY += delta[i + 1];
            }
            if (hasPath(maze, visited, new int[]{newX - delta[i], newY - delta[i + 1]}, destination)) return true;
        }
        return false;
    }
}
```

* 也可使用广度优先搜索。

```java
class Solution {
    int[] delta = new int[]{0, 1, 0, -1, 0};

    public boolean hasPath(int[][] maze, int[] start, int[] destination) {
        if (maze == null || maze.length == 0 || maze[0].length == 0) throw new IllegalArgumentException("invalid param");

        int n = maze.length, m = maze[0].length;
        // 拐点访问标识
        boolean[][] visited = new boolean[n][m];
        Queue<int[]> queue = new LinkedList<>();
        queue.add(start);

        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            if (cur[0] == destination[0] && cur[1] == destination[1]) return true;

            // 遍历四个方向
            for (int i = 0; i < 4; i++) {
                int newX = cur[0], newY = cur[1];
                while (newX >= 0 && newX < n && newY >= 0 && newY < m && maze[newX][newY] == 0) {
                    newX += delta[i];
                    newY += delta[i + 1];
                }
                newX -= delta[i];
                newY -= delta[i + 1];
                // 未加入队列则加入
                if (visited[newX][newY]) continue;
                visited[newX][newY] = true;
                queue.add(new int[]{newX, newY});
            }
        }

        return false;
    }
}
```

## 性能分析

&emsp;深度优先搜索时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：2ms，在所有java提交中击败了93.06%的用户。

内存消耗：40.4MB，在所有java提交中击败了100.00%的用户。

&emsp;广度优先搜索时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：5ms，在所有java提交中击败了52.05%的用户。

内存消耗：40.5MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。
