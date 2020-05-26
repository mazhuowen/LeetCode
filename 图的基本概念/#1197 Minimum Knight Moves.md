[toc]

In an **infinite** chess board with coordinates from $-\infty$ to $+\infty$, you have a **knight** at square `[0, 0]`.

A knight has 8 possible moves it can make, as illustrated below. Each move is two squares in a cardinal direction, then one square in an orthogonal direction.

Return the minimum number of steps needed to move the knight to the square `[x, y]`.  It is guaranteed the answer exists.



**Constraints:**

- $\vert x\vert + \vert y\vert \le 300$



## 题目解读

&emsp;骑士走到指定位置的最少步数。

```java
class Solution {
    public int minKnightMoves(int x, int y) {

    }
}
```

## 程序设计

* 最基本的思路是广度优先搜索。

```java
class Solution {
    int[] delta = new int[]{1, 2, -1, -2, 1, -2, -1, 2, 1};

    public int minKnightMoves(int x, int y) {
        int step = 0;
        // 保存遍历过的点
        Set<Integer> set = new HashSet<>();
        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{0, 0});
        set.add(0);
        // 广度优先搜索
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int[] cur = queue.poll();
                // 到达位置
                if (cur[0] == x && cur[1] == y) return step;

                for (int j = 0; j < 8; j++) {
                    int newX = cur[0] + delta[j], newY = cur[1] + delta[j + 1];
                    if (set.contains(1000 * newX + newY)) continue;
                    set.add(1000 * newX + newY);

                    queue.add(new int[]{newX, newY});
                }
            }
            step++;
        }
        return step;
    }
}
```

* 由于目标点在一个固定的方向，可以剪枝优化。

```java
class Solution {
    int[] delta = new int[]{1, 2, -1, -2, 1, -2, -1, 2, 1};

    public int minKnightMoves(int x, int y) {
        int step = 0;
        // 保存遍历过的点
        Set<Integer> set = new HashSet<>();
        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{0, 0});
        set.add(0);
        // 广度优先搜索
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int[] cur = queue.poll();
                // 到达位置
                if (cur[0] == x && cur[1] == y) return step;

                int dis = distance(cur[0], cur[1], x, y);
                for (int j = 0; j < 8; j++) {
                    int newX = cur[0] + delta[j], newY = cur[1] + delta[j + 1];
                    if (set.contains(1000 * newX + newY)) continue;
                    set.add(1000 * newX + newY);

                    // 距离剪枝
                    if (distance(newX, newY, x, y) < dis || distance(newX, newY, x, y) < 4)
                        queue.add(new int[]{newX, newY});
                }
            }
            step++;
        }
        return step;
    }

    private int distance(int curX, int curY, int targetX, int targetY) {
        return Math.abs(curX - targetX) + Math.abs(curY - targetY);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(4^{X+Y})$，空间复杂度为$O(XY)$。

执行用时：724ms，在所有java提交中击败了19.21%的用户。

内存消耗：71.1MB，在所有java提交中击败了50.00%的用户。

&emsp;剪枝后性能如下：

执行用时：102ms，在所有java提交中击败了63.55%的用户。

内存消耗：46.7MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区采用快速逼近目标值，然后进行广度有限搜索。

```java
class Solution {
    int[] delta = new int[]{1, 2, -1, -2, 1, -2, -1, 2, 1};

    public int minKnightMoves(int x, int y) {
        x = Math.abs(x);
        y = Math.abs(y);
        // 保存遍历过的点
        Set<Integer> set = new HashSet<>();
        Queue<int[]> queue = new LinkedList<>();

        int[] start = fastMove(x, y);
        int step = start[2];
        queue.add(new int[]{start[0], start[1]});
        set.add(1000 * start[0] + start[1]);
        // 广度优先搜索
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int[] cur = queue.poll();
                // 到达位置
                if (cur[0] == x && cur[1] == y) return step;

                int dis = distance(cur[0], cur[1], x, y);
                for (int j = 0; j < 8; j++) {
                    int newX = cur[0] + delta[j], newY = cur[1] + delta[j + 1];
                    if (set.contains(1000 * newX + newY)) continue;
                    set.add(1000 * newX + newY);

                    // 距离剪枝
                    if (distance(newX, newY, x, y) < dis || distance(newX, newY, x, y) < 4)
                        queue.add(new int[]{newX, newY});
                }
            }
            step++;
        }
        return step;
    }

    // 快速逼近
    private int[] fastMove(int x, int y) {
        int step = 0;
        int i = 0, j = 0;
        // 将距离缩短在目标范围4以内（由于走两步，此处为5）
        while (x - i >= 5 || y - j >= 5) {
            if (x - i >= y - j) {
                i += 2;
                // y - j不能小于2，避免j跳两步时超出范围
                j += y - j > 2 ? 1 : -1;
            } else {
                j += 2;
                i += x - i > 2 ? 1 : -1;
            }
            step++;
        }
        return new int[]{i, j, step};
    }

    private int distance(int curX, int curY, int targetX, int targetY) {
        return Math.abs(curX - targetX) + Math.abs(curY - targetY);
    }
}
```

执行用时：2ms，在所有java提交中击败了95.07%的用户。

内存消耗：36.3MB，在所有java提交中击败了100.00%的用户。