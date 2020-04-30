[toc]

On an $N \times N$ chessboard, a knight starts at the $r$-th row and $c$-th column and attempts to make exactly $K$ moves. The rows and columns are 0 indexed, so the top-left square is $(0, 0)$, and the bottom-right square is $(N-1, N-1)$.

A chess knight has 8 possible moves it can make, as illustrated below. Each move is two squares in a cardinal direction, then one square in an orthogonal direction.

<img src="../images/#688.png" style="zoom:80%;" />

Each time the knight is to move, it chooses one of eight possible moves uniformly at random (even if the piece would go off the chessboard) and moves there.

The knight continues moving until it has made exactly $K$ moves or has moved off the chessboard. Return the probability that the knight remains on the board after it has stopped moving.



Note:

* $N$ will be between $1$ and $25$.
* $K$ will be between $0$ and $100$.
* The knight always initially starts on the board.



## 题目解读

&emsp;求马走$K$步后还在棋盘上的概率。

```java
class Solution {
    public double knightProbability(int N, int K, int r, int c) {

    }
}
```

## 程序设计

* 类似广度优先有所的思路，从第0步开始模拟所有情况，使用三维动态规划数组实现。

```java
class Solution {
    int[] delta = new int[]{-2, 1, -2, -1, 2, 1, 2, -1, -2};

    public double knightProbability(int N, int K, int r, int c) {
        // 记录第k步，棋子移动到(i,j)的概率
        double[][][] dp = new double[N][N][K + 1];
        // 在起始位置的概率
        dp[r][c][0] = 1.0D;

        for (int k = 0; k < K; k++) {
            for (int i = 0; i < N; i++) {
                for (int j = 0; j < N; j++) {
                    if (dp[i][j][k] == 0) continue;

                    for (int l = 0; l < 8; l++) {
                        int x = i + delta[l], y = j + delta[l + 1];
                        if (x < 0 || y < 0 || x >= N || y >= N) continue;
						
                        // 从原地址移动到新地址概率是八分之一
                        dp[x][y][k + 1] += dp[i][j][k] / 8;
                    }
                }
            }
        }
        // 计算第k步所有位置的概率和
        double p = 0;
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                p += dp[i][j][K];
            }
        }

        return p;
    }
}
```

* 简化数组为二维得：

```java
class Solution {
    int[] delta = new int[]{-2, 1, -2, -1, 2, 1, 2, -1, -2};

    public double knightProbability(int N, int K, int r, int c) {
        double[][] dp = new double[N][N];
        dp[r][c] = 1.0D;

        for (int k = 0; k < K; k++) {
            double[][] temp = new double[N][N];
            for (int i = 0; i < N; i++) {
                for (int j = 0; j < N; j++) {
                    if (dp[i][j] == 0) continue;

                    for (int l = 0; l < 8; l++) {
                        int x = i + delta[l], y = j + delta[l + 1];
                        if (x < 0 || y < 0 || x >= N || y >= N) continue;

                        temp[x][y] += dp[i][j] / 8;
                    }
                }
            }
            dp = temp;
        }
        double p = 0;
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                p += dp[i][j];
            }
        }

        return p;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(KN^2)$，空间复杂度为$O(KN^2)$。

执行用时：5ms，在所有java提交中击败了65.27%的用户。

内存消耗：37.5MB，在所有java提交中击败了100.00%的用户。

&emsp;简化空间后，时间复杂度为$O(KN^2)$，空间复杂度为$O(N^2)$。

执行用时：5ms，在所有java提交中击败了65.27%的用户。

内存消耗：37.1MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。