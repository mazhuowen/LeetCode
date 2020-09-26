[toc]

我们得到了一副藏宝图，藏宝图显示，在一个迷宫中存在着未被世人发现的宝藏。

迷宫是一个二维矩阵，用一个字符串数组表示。它标识了唯一的入口（用 'S' 表示），和唯一的宝藏地点（用 'T' 表示）。但是，宝藏被一些隐蔽的机关保护了起来。在地图上有若干个机关点（用 'M' 表示），**只有所有机关均被触发，才可以拿到宝藏**。

要保持机关的触发，需要把一个重石放在上面。迷宫中有若干个石堆（用 'O' 表示），每个石堆都有**无限**个足够触发机关的重石。但是由于石头太重，我们一次只能搬一个石头到指定地点。

迷宫中同样有一些墙壁（用 '#' 表示），我们不能走入墙壁。剩余的都是可随意通行的点（用 '.' 表示）。石堆、机关、起点和终点（无论是否能拿到宝藏）也是可以通行的。

我们每步可以选择向上/向下/向左/向右移动一格，并且不能移出迷宫。搬起石头和放下石头不算步数。那么，从起点开始，我们最少需要多少步才能最后拿到宝藏呢？如果无法拿到宝藏，返回 -1 。



**限制**：

* $1 \le \text{maze.length} \le 100$
* $1 \le \text{maze[i].length} \le 100$
* $\text{maze[i].length} == \text{maze[j].length}$
* `S` 和 `T` 有且只有一个
* $0 \le \text{M的数量} \le 16$
* $0 \le \text{O的数量} \le 40$，题目保证当迷宫中存在$M$时，一定存在至少一个$O$。



## 题目解读

&emsp;求到达终点所需最少步数，无法到达则返回$-1$。题目规定到达终点必须先经过所有机关，而每个机关都需要石头才能打开。

```java
class Solution {
    public int minimalSteps(String[] maze) {

    }
}
```

## 程序设计

* 参考官方解题，路径主要分起始点经过某个石堆到达机关、机关经过某个石堆到达机关、机关到达终结点三种情况，给定矩阵后两两距离是固定的，可提前计算，然后动态规划求解：
  * 首先计算起始点、终结点、机关点到矩阵各个点的距离；如果没有机关，则直接返回起始点到终结点的距离；如果没有石头堆，则直接返回`-1`；
  * 根据初步计算的距离，计算各机关点之间（包含石头堆）的最短距离，即遍历机关一到各个石头堆与该石头堆到机关二的距离之和，取最小值；其中起始点到机关同理，而机关到终结点无需石头堆，直接取距离即可；
  * 在得到距离后，问题就转化为路径压缩动态规划，`dp(i,j)`表示当前经过的机关状态为$i$，最后一个机关为$j$，则根据机关$j$更新到下一个机关$k$的距离。

```java
class Solution {
    private static final int[] delta = new int[]{1, 0, -1, 0, 1};
    // 起始结束点坐标
    int sx = -1, sy = -1, tx = -1, ty = -1;
    // 机关和石头堆坐标
    List<int[]> M, O;

    public int minimalSteps(String[] maze) {
        preProcess(maze);

        // 计算距离
        int[][] startDis = bfs(sx, sy, maze);
        int mNum = M.size(), oNum = O.size();
        // 没有机关，则返回起始到终点的距离
        if (mNum == 0) return startDis[tx][ty];
        // 有机关但没有石头，返回-1
        if (oNum == 0) return -1;

        int[][] dis = distance(maze, startDis);

        // 存在不可达（当前机关与起始点、终点不连通，则无法到达）
        for (int i = 0; i < mNum; i++) {
            if (dis[i][mNum] == Integer.MAX_VALUE || dis[i][mNum + 1] == Integer.MAX_VALUE) return -1;
        }

        // 路径压缩动态规划dp(i,j)表示经过的机关状态为i，当前在机关j的最短路径
        int[][] dp = new int[1 << mNum][mNum];
        for (int[] arr : dp) Arrays.fill(arr, Integer.MAX_VALUE);
        // 初始化起点到各个机关的距离
        for (int i = 0; i < mNum; i++) dp[1 << i][i] = dis[i][mNum];
        // 遍历状态stat，当前在i的情况，得到在j的情况
        for (int stat = 1; stat < (1 << mNum); stat++) {
            for (int i = 0; i < mNum; i++) {
                // stat必须包含i
                if (((stat >> i) & 1) == 0) continue;
                for (int j = 0; j < mNum; j++) {
                    // 已经到达
                    if (((stat >> j) & 1) == 1) continue;
                    dp[stat | (1 << j)][j] = Math.min(dp[stat | (1 << j)][j], dp[stat][i] + dis[i][j]);
                }
            }
        }

        int res = Integer.MAX_VALUE;
        for (int i = 0; i < mNum; i++) {
            if (dp[(1 << mNum) - 1][i] != Integer.MAX_VALUE) res = Math.min(res, dp[(1 << mNum) - 1][i] + dis[i][mNum + 1]);
        }

        return res == Integer.MAX_VALUE ? -1 : res;
    }

    private void preProcess(String[] maze) {
        int n = maze.length, m = maze[0].length();
        M = new LinkedList<>();
        O = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                char c = maze[i].charAt(j);
                if (c == 'S') {
                    sx = i;
                    sy = j;
                } else if (c == 'T') {
                    tx = i;
                    ty = j;
                } else if (c == 'M') {
                    M.add(new int[]{i, j});
                } else if (c == 'O') {
                    O.add(new int[]{i, j});
                }
            }
        }
    }

    // 计算从机关到石堆再到其他机关或起始、钟结点的最短距离
    private int[][] distance(String[] maze, int[][] startDis) {
        int mNum = M.size();
        // 记录机关到所有其他点的距离，中间计算结果
        int[][][] tmp = new int[mNum][][];
        for (int i = 0; i < mNum; i++) tmp[i] =  bfs(M.get(i)[0], M.get(i)[1], maze);
        
        // 从机关到石堆再到其他机关或起始、钟结点的最短距离
        int[][] dis = new int[mNum][mNum + 2];
        for (int i = 0; i < mNum; i++) {
            // 到达起始点的最短距离
            dis[i][mNum] = Integer.MAX_VALUE;
            for (int[] stone : O) {
                // 当前机关及起始点不可达石头，则继续
                if (tmp[i][stone[0]][stone[1]] == -1 || startDis[stone[0]][stone[1]] == -1) continue;
                // 最短距离，更新
                dis[i][mNum] = Math.min(dis[i][mNum], tmp[i][stone[0]][stone[1]] + startDis[stone[0]][stone[1]]);
            }

            // 到达终点的最短距离，注意机关到达终点无需在经过石头
            dis[i][mNum + 1] = tmp[i][tx][ty] == -1 ? Integer.MAX_VALUE : tmp[i][tx][ty];

            // 到达其他机关的距离（注意避免重复计算）
            for (int j = i + 1; j < mNum; j++) {
                dis[i][j] = Integer.MAX_VALUE;
                for (int[] stone : O) {
                    // 机关不可达石头，则继续
                    if (tmp[i][stone[0]][stone[1]] == -1 || tmp[j][stone[0]][stone[1]] == -1) continue;
                    dis[i][j] = Math.min(dis[i][j], tmp[i][stone[0]][stone[1]] + tmp[j][stone[0]][stone[1]]);
                }
                dis[j][i] = dis[i][j];
            }
        }

        return dis;
    }

    // 广度优先求解距离
    private int[][] bfs(int x, int y, String[] maze) {
        int n = maze.length, m = maze[0].length();
        int[][] dis = new int[n][m];
        for (int[] arr : dis) Arrays.fill(arr, -1);
        dis[x][y] = 0;

        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{x, y});
        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            for (int i = 0; i < 4; i++) {
                int newX = cur[0] + delta[i], newY = cur[1] + delta[i + 1];
                // 坐标不合法或是障碍物或已遍历，则跳过
                if (newX < 0 || newX >= n || newY < 0 || newY >= m
                     || maze[newX].charAt(newY) == '#' || dis[newX][newY] != -1) continue;
                // 更新距离
                dis[newX][newY] = dis[cur[0]][cur[1]] + 1;
                queue.add(new int[]{newX, newY});
            }
        }
        return dis;
    }
}
```

## 性能分析

&emsp;时间复杂度预处理为$O(NM)$，计算各机关到其他点的距离为$O(LNM)$，计算机关与机关间经过石堆的最短距离为$O(L\max(L,O))$，动态规划花费$O(L^22^L)$，时间复杂度为其中最大值；空间复杂度为$O(\max(NM,LNM,L2^L))$，其中机关数目为$L$，石头数目为$O$。

执行用时：75ms，在所有java提交中击败了97.36%的用户。

内存消耗：43.1MB，在所有java提交中击败了82.66%的用户。

## 官方解题

&emsp;上述思路参考官方解题。