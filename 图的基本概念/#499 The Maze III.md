[toc]

There is a **ball** in a maze with empty spaces and walls. The ball can go through empty spaces by rolling **up** (u), **down** (d), **left** (l) or **right** (r), but it won't stop rolling until hitting a wall. When the ball stops, it could choose the next direction. There is also a **hole** in this maze. The ball will drop into the hole if it rolls on to the hole.

Given the **ball position**, the **hole position** and the **maze**, find out how the ball could drop into the hole by moving the **shortest distance**. The distance is defined by the number of **empty spaces** traveled by the ball from the start position (excluded) to the hole (included). Output the moving **directions** by using 'u', 'd', 'l' and 'r'. Since there could be several different shortest ways, you should output the **lexicographically smallest** way. If the ball cannot reach the hole, output "impossible".

The maze is represented by a binary 2D array. 1 means the wall and 0 means the empty space. You may assume that the borders of the maze are all walls. The ball and the hole coordinates are represented by row and column indexes.



Note:

* There is only one ball and one hole in the maze.
* Both the ball and hole exist on an empty space, and they will not be at the same position initially.
* The given maze does not contain border (like the red rectangle in the example pictures), but you could assume the border of the maze are all walls.
* The maze contains at least 2 empty spaces, and the width and the height of the maze won't exceed 30.



## 题目解读

&emsp;返回球到达球洞的最短路径。不同于[#505 The Maze II](./#505 The Maze II.md)，球洞可以在路径中，需要对路径中的点判断；其次需要给出最短路径，多个最短路径则取字典序小的。

```java
class Solution {
    public String findShortestWay(int[][] maze, int[] ball, int[] hole) {

    }
}
```

## 程序设计

* 首先需要判断滚动到下个拐点的路径中是否存在洞，存在则提前结束路径；其次对于路径字符串可用额外空间保存并比较更新。

```java
class Solution {
    // d、l、r、u
    int[][] delta = new int[][]{
            {1, 0},
            {0, -1},
            {0, 1},
            {-1, 0}
    };
    String[] map = new String[]{"d", "l", "r", "u"};

    public String findShortestWay(int[][] maze, int[] ball, int[] hole) {
        if (maze == null || maze.length == 0 || maze[0].length == 0) throw new IllegalArgumentException("invalid param");

        int n = maze.length, m = maze[0].length;
        // 距离
        int[][] distance = new int[n][m];
        for (int[] array : distance) Arrays.fill(array, Integer.MAX_VALUE);
        // 路径记录
        String[][] path = new String[n][m];

        Queue<int[]> queue = new LinkedList<>();
        queue.add(ball);
        // 初始化
        distance[ball[0]][ball[1]] = 0;
        path[ball[0]][ball[1]] = "";

        while (!queue.isEmpty()) {
            int[] cur = queue.poll();

            // 从拐点尝试四个方向
            for (int i = 0; i < 4; i++) {
                int[] dir = delta[i];
                int newX = cur[0], newY = cur[1], count = 0;
                // 判断路径中是否存在洞
                boolean complete = false;
                while (newX >= 0 && newX < n && newY >= 0 && newY < m && maze[newX][newY] == 0 && !complete) {
                    if (newX == hole[0] && newY == hole[1]) complete = true;
                    newX += dir[0];
                    newY += dir[1];
                    count++;
                }
                // 得到拐点或洞的位置
                newX -= dir[0];
                newY -= dir[1];
                count--;
                // 路径更短或字典序更小，则更新
                if (distance[cur[0]][cur[1]] + count < distance[newX][newY]
                    || distance[cur[0]][cur[1]] + count == distance[newX][newY] && path[newX][newY].compareTo(path[cur[0]][cur[1]] + map[i]) > 0) {
                    distance[newX][newY] = distance[cur[0]][cur[1]] + count;
                    path[newX][newY] = path[cur[0]][cur[1]] + map[i];
                    // 加入队列
                    if (!complete) queue.add(new int[]{newX, newY});
                }
            }
        }

        return path[hole[0]][hole[1]] == null ? "impossible" : path[hole[0]][hole[1]];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN\max(M,N))$，空间复杂度为$O(MN)$。

执行用时：9ms，在所有java提交中击败了84.87%的用户。

内存消耗：39.2MB，在所有java提交中击败了66.67%的用户。

## 官方解题

&emsp;暂无，密切关注。参考社区思路，采用优先级队列保存路径，每次选择最小的路径进行迭代。

```java
class Solution {
    int[] delta = new int[]{0, 1, 0, -1, 0};
    String[] direct = new String[]{"r", "d", "l", "u"};

    public String findShortestWay(int[][] maze, int[] ball, int[] hole) {
        if (maze == null || maze.length == 0 || maze[0].length == 0) throw new IllegalArgumentException("invalid param");

        int n = maze.length, m = maze[0].length;
        boolean[][] visited = new boolean[n][m];
        PriorityQueue<Path> queue = new PriorityQueue<>();
        queue.add(new Path(ball[0], ball[1], 0, ""));

        while (!queue.isEmpty()) {
            Path cur = queue.poll();
            // 此时到达拐点的路径最小，标记为已访问
            if (visited[cur.x][cur.y]) continue;
            visited[cur.x][cur.y] = true;
            if (cur.x == hole[0] && cur.y == hole[1]) return cur.path;

            for (int i = 0; i < 4; i++) {
                // 到达下一个拐点
                int newX = cur.x, newY = cur.y, count = 0;
                boolean complete = false;
                while (newX >= 0 && newX < n && newY >= 0 && newY < m && maze[newX][newY] == 0 && !complete) {
                    if (newX == hole[0] && newY == hole[1]) complete = true;
                    newX += delta[i];
                    newY += delta[i + 1];
                    count++;
                }

                newX -= delta[i];
                newY -= delta[i + 1];
                count--;
                if (visited[newX][newY]) continue;
                queue.add(new Path(newX, newY, cur.count + count, cur.path + direct[i]));
            }
        }

        return "impossible";
    }
}

class Path implements Comparable<Path> {
    int x, y;
    int count;
    String path;

    Path(int x, int y, int count, String path) {
        this.x = x;
        this.y = y;
        this.count = count;
        this.path = path;
    }

    @Override
    public int compareTo(Path that) {
        return this.count == that.count ? this.path.compareTo(that.path) : this.count - that.count;
    }
}
```

执行用时：6ms，在所有java提交中击败了93.28%的用户。

内存消耗：39.5MB，在所有java提交中击败了33.33%的用户。