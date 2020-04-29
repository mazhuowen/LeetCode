[toc]

You are given a $m \times n$ 2D grid initialized with these three possible values.

* `-1` - A wall or an obstacle.
* `0` - A gate.
* `INF` - Infinity means an empty room. We use the value $2^{31} - 1 = 2147483647$ to represent `INF` as you may assume that the distance to a gate is less than `2147483647`.

Fill each empty room with the distance to its nearest gate. If it is impossible to reach a gate, it should be filled with `INF`.



## 题目解读

&emsp;找到房间中里门最近的距离。

```java
class Solution {
    public void wallsAndGates(int[][] rooms) {

    }
}
```

## 程序设计

* 首先想到的是从门开始深度优先搜索。

```java
class Solution {
    int[] delta = new int[]{0, 1, 0, -1, 0};

    public void wallsAndGates(int[][] rooms) {
        if (rooms == null || rooms.length == 0 || rooms[0].length == 0) return;

        int m = rooms.length, n = rooms[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (rooms[i][j] != 0) continue;
                wallsAndGates(rooms, i, j, 0);
            }
        }  
    }

    // dis为当前距起始的门的距离
    private void wallsAndGates(int[][] rooms, int i, int j, int dis) {
        // 坐标不符合要求或距离小于当前距离（墙或有更好的路径）
        if (i < 0 || i >= rooms.length || j < 0 || 
            j >= rooms[0].length || rooms[i][j] < dis) return;

        // 记录路径
        rooms[i][j] = dis;
        // 深度优先搜索
        for (int k = 0; k < 4; k++) {
            wallsAndGates(rooms, i + delta[k], j + delta[k + 1], dis + 1);
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(M^2N^2)$，空间复杂度为$O(MN)$。

执行用时：7ms，在所有java提交中击败了90.16%的用户。

内存消耗：44.8MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;官方提供了广度优先搜索的思路。

```java
class Solution {
    int[] delta = new int[]{0, 1, 0, -1, 0};

    public void wallsAndGates(int[][] rooms) {
        if (rooms == null || rooms.length == 0 || rooms[0].length == 0) return;

        // 将门加入队列
        Queue<int[]> queue = new LinkedList<>();
        int m = rooms.length, n = rooms[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (rooms[i][j] != 0) continue;
                queue.add(new int[]{i, j});
            }
        }

        // 广度优先遍历
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int[] cur = queue.poll();
                int x = cur[0], y = cur[1];

                for (int j = 0; j < 4; j++) {
                    if (x + delta[j] < 0 || x + delta[j] >= m || y + delta[j + 1] < 0 || y + delta[j + 1] >= n || rooms[x + delta[j]][ y + delta[j + 1]] != Integer.MAX_VALUE) continue;
                    queue.add(new int[]{x + delta[j], y + delta[j + 1]});
                    // 较为关键，避免了队列中重复坐标的影响
                    rooms[x + delta[j]][ y + delta[j + 1]] = rooms[x][y] + 1;
                }
            }
        }
    }
}
```

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：8ms，在所有java提交中击败了87.17%的用户。

内存消耗：43.2MB，在所有java提交中击败了90.00%的用户。