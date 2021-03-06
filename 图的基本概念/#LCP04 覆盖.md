[toc]

有一块棋盘，棋盘上有一些格子已经坏掉了；还有无穷块大小为$1 \times 2$的多米诺骨牌，想把这些骨牌**不重叠**地覆盖在**完好**的格子上，请找出最多能在棋盘上放置的骨牌数，这些骨牌可以横着或者竖着放。

输入：$n$, $m$代表棋盘的大小；`broken`是一个$b \times 2$的二维数组，其中每个元素代表棋盘上每一个坏掉的格子的位置。

输出：一个整数，代表最多能在棋盘上放的骨牌数。

<img src="../images/#lcp04.jpg"  />



**限制：**

* $1 \le n \le 8$
* $1 \le m \le 8$
* $0 \le b \le n \times m$



## 题目解读

&emsp;求最多能在棋盘上放的骨牌数。

```java
class Solution {
    public int domino(int n, int m, int[][] broken) {

    }
}
```

## 程序设计

* 骨牌只能放在两个格子上，放置后其他骨牌无法放置；可看作是两个格子配对，问题转化为最大匹配问题，即匈牙利算法；
* 每个格子看作是一个节点，只与相邻的四个格子相连，可知这样的图满足二分图的特性，可以采用匈牙利算法。

```java
class Solution {
    int[] delta = new int[]{1, 0, -1, 0, 1};

    public int domino(int n, int m, int[][] broken) {

        Node[] graph = new Node[n * m];
        // 标记坏掉的格子
        for (int[] b : broken) {
            graph[b[0] * m + b[1]] = new Node(-1, null);
        }

        // 构建图
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                int ver = i * m + j;
                if (graph[ver] != null && graph[ver].ver == -1) continue;
                for (int k = 0; k < 4; k++) {
                    int x = i + delta[k], y = j + delta[k + 1];
                    if (x < 0 || x >= n || y < 0 || y >= m) continue;
                    // 当前为损坏点，跳过
                    if (graph[x * m + y] != null && graph[x * m + y].ver == -1) continue;
                    graph[ver] = new Node(x * m + y, graph[ver]);
                }
            }
        }

        return hungarian(graph);
    }

    private int hungarian(Node[] graph) {
        int res = 0;

        int n = graph.length;
        int[] matched = new int[n];
        Arrays.fill(matched, -1);
        for (int i = 0; i < n; i++) {
            // 已匹配或坏掉的格子不搜索
            if (matched[i] != -1 || (graph[i] != null && graph[i].ver == -1)) continue;

            // 对未匹配的节点进行搜索
            boolean[] visited = new boolean[n];
            // 从每个点开始进行增广路径扩展，若匹配成功，则匹配边数加一
            if (dfs(graph, i, visited, matched)) res++;
        }
        return res;
    }

    private boolean dfs(Node[] graph, int i, boolean[] visited, int[] matched) {
        for (Node temp = graph[i]; temp != null; temp = temp.next) {
            if (visited[temp.ver]) continue;
            visited[temp.ver] = true;
            
            // 若遇到未匹配点，则匹配；若遇到匹配点则进行增广路径拓展，如果可以拓展，则翻转沿途的匹配边
            if (matched[temp.ver] == -1 || dfs(graph, matched[temp.ver], visited, matched)) {
                matched[temp.ver] = i;
                matched[i] = temp.ver;
                return true;
            }
        }
        // 增广路径无法匹配，无法重新挪出新位置，返回
        return false;
    }
}

class Node {
    int ver;
    Node next;

    public Node(int ver, Node next) {
        this.ver = ver;
        this.next = next;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(VE)$，空间复杂度为$O(VE)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.8MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区还有使用轮廓线动态规划的方法。

```java
class Solution {
    int n, m, mod;
    boolean[][] mark;
    int[][][] dp;

    public int domino(int n, int m, int[][] broken) {
        this.n = n;
        this.m = m;
        this.mod = (1 << (m - 1)) - 1;
        // 标记损毁的棋盘位置
        this.mark = new boolean[n][m];
        for (int[] grid : broken) mark[grid[0]][grid[1]] = true;
        // 轮廓线动态规划数组，分别表示x、y和之前的m个棋盘位置状态
        this.dp = new int[n][m][1 << m];
        return backTracing(0, 0, 0);
    }

    private int backTracing(int x, int y, int preStat) {
        if (x == n) return 0;
        if (y == m) return backTracing(x + 1, 0, preStat);
        if (dp[x][y][preStat] != 0) return dp[x][y][preStat];

        // 当前位置上方和左侧状态
        int up = (preStat & (1 << (m - 1))) >> (m - 1), left = preStat & 1;
        // 当前棋盘不连接
        int res = backTracing(x, y + 1, (preStat & mod) << 1);
        // 如果上方可用，连接当前棋盘和上方棋盘
        if (!mark[x][y] && x > 0 && !mark[x - 1][y] && up == 0) {
            // 将当前棋盘状态设置为1，上方已出队，不再关注
            res = Math.max(res, 1 + backTracing(x, y + 1, (preStat & mod) << 1 | 1));
        }
        // 如果左侧可用，连接当前和左侧
        if (!mark[x][y] && y > 0 && !mark[x][y - 1] && left == 0) {
            // 将当前和左侧设置为1
            res = Math.max(res, 1 + backTracing(x, y + 1, (preStat & mod) << 1 | 3));
        }
        return dp[x][y][preStat] = res;
    }
}
```

&emsp;时间复杂度为$O(MN2^M)$，空间复杂度为$O(MN2^M)$。

执行用时：2 ms, 在所有 Java 提交中击败了72.97%的用户。

内存消耗：38.1 MB, 在所有 Java 提交中击败了33.33%的用户。