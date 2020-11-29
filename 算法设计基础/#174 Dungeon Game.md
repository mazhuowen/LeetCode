[toc]

The demons had captured the princess (**P**) and imprisoned her in the bottom-right corner of a dungeon. The dungeon consists of $M \times N$ rooms laid out in a 2D grid. Our valiant knight (**K**) was initially positioned in the top-left room and must fight his way through the dungeon to rescue the princess.

The knight has an initial health point represented by a positive integer. If at any point his health point drops to 0 or below, he dies immediately.

Some of the rooms are guarded by demons, so the knight loses health (negative integers) upon entering these rooms; other rooms are either empty (0's) or contain magic orbs that increase the knight's health (positive integers).

In order to reach the princess as quickly as possible, the knight decides to move only rightward or downward in each step.

Write a function to determine the knight's minimum initial health so that he is able to rescue the princess**.

For example, given the dungeon below, the initial health of the knight must be at least 7 if he follows the optimal path `RIGHT-> RIGHT -> DOWN -> DOWN`.

| -2(K) | -3   | 3     |
| ----- | ---- | ----- |
| -5    | -10  | 1     |
| 10    | -30  | -5(P) |



**Note**:

* The knight's health has no upper bound.
* Any room can contain threats or power-ups, even the first room the knight enters and the bottom-right room where the princess is imprisoned.



## 题目解读

&emsp;`K`表示骑士，`P`表示公主，每一格表示骑士的生命变化，某一刻骑士生命低于等于$0$就会立刻死亡。题目需要给出成功救出公主的最低初始生命。题目规定骑士只能右移和下移。

```java
class Solution {
    public int calculateMinimumHP(int[][] dungeon) {
        
    }
}
```

## 程序设计

* 若以起始点开始遍历并动态规划保存当前位置需要的最小生命值，若当前格子使得骑士减少生命值（或不变），假设改变值为$K$，则$K \le 0$。倘若之前格子（不管是上面还是左边）所需的最少生命值为$X$，则当前格子需要$X - K$的生命值；若$K > 0$则当前格子所需生命值和前一格子所需生命值一致，为$X$。这样我们就得出了递推公式$X_{t + 1} = \max(X_t, X_t - K_{t + 1})$。
* 但是上面说的前一个格子有两种情况，即左边和上面，怎么选择才是最优的呢？以`[[0,0,0],[-1,0,0],[2,0,-2]]`为例，最后一行的`0`上面的格子`0`所需最小生命值为1，而左边格子`2`所需生命值为2，如果单纯的根据前一个格子的最小生命值更新当前结点，则得到`0`所需生命值为1，最后终点`-2`所需生命值为3，显然如果`0`选择`2`这条路径，得到的结果是2，最优解。
* 可以看到从起始点开始遍历，由于不确定后续路径的值，对当前格子的选择更新造成了困难。如果从终点向前遍历，则变成已知后继结点到终点的最小生命值，求解当前点的生命值，在选择时只需选择最小生命值的后继结点即可，问题迎刃而解。
* 如果反向遍历，则递推公式发生变化。假设后继结点所需最小生命值为$X_{t + 1}$，必然大于等于1，当前改变值为$K_{t}$，如果$K_t > X_{t + 1}$，则$X_t = 1$即可；如果$0 < K_t < X_{t + 1}$，则需要补充$X_{t + 1} - K_t$即可，即$X_t = X_{t + 1} - K_t$；如果$K_t \le 0$，则首先补充$-K_t$，使得生命值变化为0，然后补充$X_{t + 1}$，使得可以满足后继结点的要求，即$X_t = X_{t + 1} - K_t$，合并上述情况得$X_{t} = \max(1, X_{t + 1} - K_t)$。假设两个后继结点下方、右侧结点所需生命值分别$X_l$、$X_r$，根据上述公式，选择最小的后继得到的当前值也是较小的。这样动态规划形式如下：

```java
class Solution {
    public int calculateMinimumHP(int[][] dungeon) {
        if (dungeon == null || dungeon.length == 0) return 0;

        int m = dungeon.length, n = dungeon[0].length;
        // 动态规划数组，从m-1，n-1开始向起点靠拢
        int[][] dp = new int[m][n];

        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                // 起点
                if (i == m - 1 && j == n - 1) dp[i][j] = Math.max(1, 1 - dungeon[i][j]);
                // 行
                else if (i == m - 1) {
                    dp[i][j] = Math.max(1,  dp[i][j + 1] - dungeon[i][j]);
                }
                // 列
                else if (j == n - 1) {
                    dp[i][j] = Math.max(1,  dp[i + 1][j] - dungeon[i][j]);
                }
                // 内部，选择较小的后继更新
                else {
                    dp[i][j] = Math.max(1, Math.min(dp[i + 1][j], dp[i][j + 1]) - dungeon[i][j]);
                }
            }
        }
        return dp[0][0];
    }
}
```

* 将二维动态规划改为一维数组得：

```java
class Solution {
    public int calculateMinimumHP(int[][] dungeon) {
        if (dungeon == null || dungeon.length == 0) return 0;

        int m = dungeon.length, n = dungeon[0].length;
        // 动态规划数组，从m-1，n-1开始向起点靠拢
        int[] dp = new int[n];

        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                // 起点
                if (i == m - 1 && j == n - 1) dp[j] = Math.max(1, 1 - dungeon[i][j]);
                // 行
                else if (i == m - 1) {
                    dp[j] = Math.max(1,  dp[j + 1] - dungeon[i][j]);
                }
                // 列
                else if (j == n - 1) {
                    dp[j] = Math.max(1,  dp[j] - dungeon[i][j]);
                }
                // 内部
                else {
                    dp[j] = Math.max(1, Math.min(dp[j], dp[j + 1]) - dungeon[i][j]);
                }
            }
        }
        return dp[0];
    }
}
```

## 性能分析

&emsp;二维动态规划时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：2ms，在所有java提交中击败了96.48%的用户。

内存消耗：40.9MB，在所有java提交中击败了73.83%的用户。

&emsp;一维动态规划时间复杂度为$O(MN)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了96.48%的用户。

内存消耗：40.8MB，在所有java提交中击败了77.57%的用户。

## 官方解题

&emsp;暂无，密切关注。