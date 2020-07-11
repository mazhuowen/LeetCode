[toc]

There is a **ball** in a maze with empty spaces and walls. The ball can go through empty spaces by rolling **up**, **down**, **left** or **right**, but it won't stop rolling until hitting a wall. When the ball stops, it could choose the next direction.

Given the ball's **start position**, the **destination** and the **maze**, find the shortest distance for the ball to stop at the destination. The distance is defined by the number of **empty spaces** traveled by the ball from the start position (excluded) to the destination (included). If the ball cannot stop at the destination, return -1.

The maze is represented by a binary 2D array. 1 means the wall and 0 means the empty space. You may assume that the borders of the maze are all walls. The start and destination coordinates are represented by row and column indexes.



**Note**:

* There is only one ball and one destination in the maze.
* Both the ball and the destination exist on an empty space, and they will not be at the same position initially.
* The given maze does not contain border (like the red rectangle in the example pictures), but you could assume the border of the maze are all walls.
* The maze contains at least 2 empty spaces, and both the width and height of the maze won't exceed 100.



## 题目解读

&emsp;[#490 The Maze](./#490 The Maze.md)的进阶，求到达终点的最短距离。

```java
class Solution {
    public int shortestDistance(int[][] maze, int[] start, int[] destination) {

    }
}
```

## 程序设计

* 最短距离首先想到的是广度优先搜索。

```java
class Solution {
    int[] delta = new int[]{0, 1, 0, -1, 0};

    public int shortestDistance(int[][] maze, int[] start, int[] destination) {
        if (maze == null || maze.length == 0 || maze[0].length == 0) throw new IllegalArgumentException("invalid param");
        if (start[0] == destination[0] && start[1] == destination[1]) return 0;

        int n = maze.length, m = maze[0].length;
        boolean[][] visited = new boolean[n][m];
        Queue<int[]> queue = new LinkedList<>();
        // 添加四个方向的起始点
        queue.add(new int[]{start[0], start[1], 0});
        queue.add(new int[]{start[0], start[1], 1});
        queue.add(new int[]{start[0], start[1], 2});
        queue.add(new int[]{start[0], start[1], 3});
        visited[start[0]][start[1]] = true;
        int step = 0;

        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int[] cur = queue.poll();
                int x = cur[0], y = cur[1], direct = cur[2];
                x += delta[direct];
                y += delta[direct + 1];
                // 不是拐点，沿原方向滚动
                if (x >= 0 && x < n && y >= 0 && y < m && maze[x][y] == 0) {
                    if (!visited[x][y]) queue.add(new int[]{x, y, direct});
                }
                // 拐点，判断是否是终止点
                else {
                    x -= delta[direct];
                    y -= delta[direct + 1];
                    if (x == destination[0] && y == destination[1]) return step;

                    if (visited[x][y]) continue;
                    visited[x][y] = true;
                    // 加入四个方向的下一个位置
                    for (int j = 0; j < 4; j++) {
                        // 拐点对应下一个位置
                        int newX = x + delta[j], newY = y + delta[j + 1];
                        if (direct == j || newX < 0 || newX >= n || newY < 0 || newY >= m 
                            || maze[newX][newY] == 1) continue;
                        queue.add(new int[]{newX, newY, j});
                    }
                }
            }
            step++;
        }
        return -1;
    }
}
```

* 也可使用深度优先搜索实现，需要记录到每个拐点的步数，每次到一个拐点比较步数，如果步数大于之前步数，则终止遍历；否则更新较小步数并继续尝试。
* 在本题中，深度优先搜索存在重复遍历的问题，假设拐点在一行的中间，为$A$，可选择左右方向到达拐点$B$、$C$，此时如果选择先尝试拐点$C$，则后续必然会到达$B$，此时步数必然大于先尝试$B$的步数；而在广度优先搜索中该问题不存在，因为先入队$A$后同时遍历$B$、$C$，当$C$向$B$方向遍历的过程中必然经过$A$，由于$A$已经遍历，不会再向$B$的方向遍历，从而避免重复遍历。

```java
class Solution {
    int[] delta = new int[]{0, 1, 0, -1, 0};
    int min = Integer.MAX_VALUE;

    public int shortestDistance(int[][] maze, int[] start, int[] destination) {
        if (maze == null || maze.length == 0 || maze[0].length == 0) return -1;

        // 记录距离
        int[][] distance = new int[maze.length][maze[0].length];
        for (int[] array : distance) {
            Arrays.fill(array, Integer.MAX_VALUE);
        }
        distance(maze, distance, start, destination, 0);
        return min == Integer.MAX_VALUE ? -1 : min;
    }

    private void distance(int[][] maze, int[][] distance, int[] cur, int[] destination, int count) {
        int n = maze.length, m = maze[0].length;
        int x = cur[0], y = cur[1];

        // 剪枝
        if (count >= min || count >= distance[x][y]) return;
        distance[x][y] = count;

        if (x == destination[0] && y == destination[1]) {
            min = Math.min(min, count);
            return;
        }

        // 向四个方向运动
        for (int i = 0; i < 4; i++) {
            int newX = x, newY = y, newCount = count;
            while (newX >= 0 && newX < n && newY >= 0 && newY < m && maze[newX][newY] == 0) {
                newX += delta[i];
                newY += delta[i + 1];
                newCount++;
            }
            newX -= delta[i];
            newY -= delta[i + 1];
            newCount--;
            // 试探
            distance(maze, distance, new int[]{newX, newY}, destination, newCount);
        }
    }
}
```

## 性能分析

&emsp;广度优先搜索时间复杂度为$O(MN\max(M,N))$，空间复杂度为$O(MN)$。

执行用时：12ms，在所有java提交中击败了52.72%的用户。

内存消耗：40.4MB，在所有java提交中击败了100.00%的用户。

&emsp;深度优先搜索时间复杂度为$O(MN\max(M,N))$，空间复杂度为$O(MN)$。

执行用时：363ms，在所有java提交中击败了34.73%的用户。

内存消耗：40.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方提供广度优先搜索方式与上述思路不同，任然只记录拐点，且搜索完整个矩阵。

```java
class Solution {
    int[] delta = new int[]{0, 1, 0, -1, 0};

    public int shortestDistance(int[][] maze, int[] start, int[] destination) {
        if (maze == null || maze.length == 0 || maze[0].length == 0) throw new IllegalArgumentException("invalid param");
        if (start[0] == destination[0] && start[1] == destination[1]) return 0;

        int n = maze.length, m = maze[0].length;
        // 根据距离剪枝
        int[][] distance = new int[n][m];
        for (int[] array : distance) Arrays.fill(array, Integer.MAX_VALUE);
        Queue<int[]> queue = new LinkedList<>();
        distance[start[0]][start[1]] = 0; 
        queue.add(start);

        while (!queue.isEmpty()) {
            int[] cur = queue.poll();

            for (int i = 0; i < 4; i++) {
                int newX = cur[0], newY = cur[1], count = 0;
                // 遍历到下一个拐点
                while (newX >= 0 && newX < n && newY >= 0 && newY < m && maze[newX][newY] == 0) {
                    newX += delta[i];
                    newY += delta[i + 1];
                    count++;
                }
                newX -= delta[i];
                newY -= delta[i + 1];
                count--;
                // 距离小，则更新
                if (distance[cur[0]][cur[1]] + count < distance[newX][newY]) {
                    distance[newX][newY] = distance[cur[0]][cur[1]] + count;
                    queue.add(new int[]{newX, newY});
                }
            }
        }

        return distance[destination[0]][destination[1]] == Integer.MAX_VALUE ? -1 : distance[destination[0]][destination[1]];
    }
}
```

&emsp;时间复杂度为$O(MN\max(M,N))$，空间复杂度为$O(MN)$。

执行用时：8ms，在所有java提交中击败了96.65%的用户。

内存消耗：40.5MB，在所有java提交中击败了100.00%的用户。

&emsp;官方还提供了构图，然后将问题转化为图的最短距离问题。没选择最小边，加入更新距离。
