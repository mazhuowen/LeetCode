[toc]

You are given four integers, $m$, $n$, `introvertsCount`, and `extrovertsCount`. You have an $m \times n$ grid, and there are two types of people: introverts and extroverts. There are `introvertsCount` introverts and `extrovertsCount` extroverts.

You should decide how many people you want to live in the grid and assign each of them one grid cell. Note that you **do not** have to have all the people living in the grid.

The **happiness** of each person is calculated as follows:

* Introverts **start** with $120$ happiness and **lose** $30$ happiness for each neighbor (introvert or extrovert).
* Extroverts **start** with $40$ happiness and **gain** $20$ happiness for each neighbor (introvert or extrovert).

Neighbors live in the directly adjacent cells north, east, south, and west of a person's cell.

The **grid happiness** is the **sum** of each person's happiness. Return the **maximum possible grid happiness**.

 

**Example 1**:

<img src="..\images\#1659_exp1.png"  />

```
Input: m = 2, n = 3, introvertsCount = 1, extrovertsCount = 2
Output: 240
Explanation: Assume the grid is 1-indexed with coordinates (row, column).
We can put the introvert in cell (1,1) and put the extroverts in cells (1,3) and (2,3).

- Introvert at (1,1) happiness: 120 (starting happiness) - (0 * 30) (0 neighbors) = 120
- Extrovert at (1,3) happiness: 40 (starting happiness) + (1 * 20) (1 neighbor) = 60
- Extrovert at (2,3) happiness: 40 (starting happiness) + (1 * 20) (1 neighbor) = 60
  The grid happiness is 120 + 60 + 60 = 240.
  The above figure shows the grid in this example with each person's happiness. The introvert stays in the light green cell while the extroverts live on the light purple cells.
```

**Example 2**:

```
Input: m = 3, n = 1, introvertsCount = 2, extrovertsCount = 1
Output: 260
Explanation: Place the two introverts in (1,1) and (3,1) and the extrovert at (2,1).

- Introvert at (1,1) happiness: 120 (starting happiness) - (1 * 30) (1 neighbor) = 90
- Extrovert at (2,1) happiness: 40 (starting happiness) + (2 * 20) (2 neighbors) = 80
- Introvert at (3,1) happiness: 120 (starting happiness) - (1 * 30) (1 neighbor) = 90
  The grid happiness is 90 + 80 + 90 = 260.
  Example 3:

Input: m = 2, n = 2, introvertsCount = 4, extrovertsCount = 0
Output: 240
```



**Constraints**:

* $1 \le m, n \le 5$
* $0 \le \text{introvertsCount, extrovertsCount} <= \min(m * n, 6)$



## 题目解读

&emsp;内向的人每存在一个邻居就失去$30$个幸福感，外向的人每存在一个邻居就会得到$20$个幸福感。现给定格子和若干人，求分配使得总的幸福感最大，人不必全部分配完。

```java
class Solution {
    public int getMaxGridHappiness(int m, int n, int introvertsCount, int extrovertsCount) {

    }
}
```

## 程序设计

* 参考社区思路，使状态压缩表示每一行的状态，`0`表示空置，`1`表示内向，`2`表示外向，则每个状态可使用一个数组表示，提前计算所有的状态、每个状态对应的内向的人数、外向的人数、分数，及与其他状态相邻时互相影响的分数；
* 使用记忆化数组`dp(i,j,k,l)`表示上一行状态为$i$，当前行号为$j$，还剩余$k$和$l$个人时最大幸福值，则回溯尝试当前行所有状态，并取最大值。

```java
class Solution {
    int m, n, n3;
    // 每个状态压缩的三进制表示
    int[][] mask;
    // 每个状态对应内向、外向人数
    int[] introverts, extroverts;
    // 每个状态得分
    int[] score;
    // 每个状态对于其他状态的得分增益
    int[][] gain;

    // 动态规划数组
    int[][][][] dp;

    public int getMaxGridHappiness(int m, int n, int introvertsCount, int extrovertsCount) {
        this.m = m;
        this.n = n;
        this.n3 = (int)Math.pow(3, n);
        // 预处理
        process();
        // 表示前一行状态、当前行号、剩余内向、外向人数
        this.dp = new int[n3][m + 1][introvertsCount + 1][extrovertsCount + 1];
        for (int[][][] arr1 : dp) {
            for (int[][] arr2 : arr1) {
                for (int[] arr3 : arr2) Arrays.fill(arr3, -1);
            }
        }

        return backTracing(0, 0, introvertsCount, extrovertsCount);
    }

    private int backTracing(int preStat, int curRow, int restIntroverts, int restExtroverts) {
        // 处理完
        if (curRow == m || restIntroverts + restExtroverts == 0) return 0;
        if (dp[preStat][curRow][restIntroverts][restExtroverts] != -1) return dp[preStat][curRow][restIntroverts][restExtroverts];
        int max = Integer.MIN_VALUE;
        for (int curStat = 0; curStat < n3; curStat++) {
            // 人数不够
            if (introverts[curStat] > restIntroverts || extroverts[curStat] > restExtroverts) continue;
            // 计算当前分数
            int curScore = score[curStat] + gain[curStat][preStat];
            // 回溯
            max = Math.max(max, curScore + backTracing(curStat, curRow + 1, restIntroverts - introverts[curStat], restExtroverts - extroverts[curStat]));
        }
        return dp[preStat][curRow][restIntroverts][restExtroverts] = max;
    }

    private void process() {
        // 状态数目
        int num = n3;
        this.mask = new int[num][n];
        this.introverts = new int[num];
        this.extroverts = new int[num];
        this.score = new int[num];
        this.gain = new int[num][num];

        // 初始化
        for (int stat = 0; stat < num; stat++) {
            // 生成三进制序列
            mask[stat] = ternary(stat, n);
            // 计算内向、外向的人数和分数
            init(introverts, extroverts, score, stat, mask[stat]);
        }
        // 计算每个状态相邻的增益分数
        for (int stat = 0; stat < num; stat++) {
            for (int neighbor = 0; neighbor < num; neighbor++) {
                for (int i = 0; i < n; i++) gain[stat][neighbor] += calc(mask[neighbor][i], mask[stat][i]); 
            }
        }
    }

    // 转化为三进制数组
    private int[] ternary(int num, int len) {
        int idx = 0;
        int[] res = new int[len];
        while (num > 0) {
            res[idx++] = num % 3;
            num /= 3;
        }
        return res;
    }

    private void init(int[] introverts, int[] extroverts, int[] score, int mask, int[] stat) {
        for (int i = 0; i < stat.length; i++) {
            // 内向者
            if (stat[i] == 1) {
                introverts[mask]++;
                score[mask] += 120;
            }
            // 外向者
            else if (stat[i] == 2) {
                extroverts[mask]++;
                score[mask] += 40;
            }
            // 计算与前一个人互相影响的分数
            if (i > 0) score[mask] += calc(stat[i - 1], stat[i]);
        }
    }

    private int calc(int preStat, int curStat) {
        if (curStat == 0 || preStat == 0) return 0;
        // 外向和内线减少10
        if (preStat + curStat == 3) return -10;
        // 两个内向减少60
        else if (preStat + curStat == 2) return -60;
        // 两个外向增加40
        else return 40;
    }
}
```

## 性能分析

&emsp;预处理时间复杂度为$O(N3^N + N3^{2N})$，回溯阶段时间复杂度为$O(Mxy3^{2N})$ ，总的时间复杂度为$O((N + Mxy)3^{2N})$，其中$x = \text{introvertsCount}$，$y = \text{extrovertsCount}$。空间复杂度为$O(3^{2N})$。

执行用时：218 ms, 在所有 Java 提交中击败了63.39%的用户。

内存消耗：38 MB, 在所有 Java 提交中击败了89.01%的用户。

## 官方解题

&emsp;暂无，密切关注。社区有采用轮廓线状态压缩动态规划的思路，这样每次只需考虑一个位置与其上、其左的得分，简化流程。

<img src="..\images\#1659.png" style="zoom: 35%;" />

```java
class Solution {
    int m, n;
    int mod;
    int[][][][][] dp;

    public int getMaxGridHappiness(int m, int n, int introvertsCount, int extrovertsCount) {
        this.m = m;
        this.n = n;
        // 前n个状态需要出队，取余乘3即可
        this.mod = (int)Math.pow(3, n - 1);
        // 分别表示横坐标、纵坐标、内向、外向剩余人数、当前位置前n个位置的状态
        this.dp = new int[m][n][introvertsCount + 1][extrovertsCount + 1][mod * 3];
        return backTracing(0, 0, introvertsCount, extrovertsCount, 0);
    }

    private int backTracing(int x, int y, int in, int ex, int preStat) {
        // 遍历结束
        if (x == m) return 0;
        // 行遍历完，定位到下一行
        if (y == n) return backTracing(x + 1, 0, in, ex, preStat);
        if (dp[x][y][in][ex][preStat] != 0) return dp[x][y][in][ex][preStat];

        // 其上、左状态（如果存在）
        int up = preStat / mod, left = preStat % 3;
        // 当前位置空置
        int res = backTracing(x, y + 1, in, ex, preStat % mod * 3 + 0);
        // 当前位置分配内向者
        if (in > 0) {
            int score = 120;
            if (x > 0) score += calc(up, 1);
            if (y > 0) score += calc(left, 1);
            res = Math.max(res, score + backTracing(x, y + 1, in - 1, ex, preStat % mod * 3 + 1));
        }
        // 当前位置分配外向者
        if (ex > 0) {
            int score = 40;
            if (x > 0) score += calc(up, 2);
            if (y > 0) score += calc(left, 2);
            res = Math.max(res, score + backTracing(x, y + 1, in, ex - 1, preStat % mod * 3 + 2));
        }
		// 取最大值
        return dp[x][y][in][ex][preStat] = res;
    }

    private int calc(int preStat, int curStat) {
        if (curStat == 0 || preStat == 0) return 0;
        // 外向和内线减少10
        if (preStat + curStat == 3) return -10;
        // 两个内向减少60
        else if (preStat + curStat == 2) return -60;
        // 两个外向增加40
        else return 40;
    }
}
```

&emsp;时间复杂度为$O(MNxy3^N)$，空间复杂度为$O(MNxy3^N)$。

执行用时：50 ms, 在所有 Java 提交中击败了98.91%的用户。

内存消耗：38.4 MB, 在所有 Java 提交中击败了80.22%的用户。