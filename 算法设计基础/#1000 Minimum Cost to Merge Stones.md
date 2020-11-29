[toc]

There are `N` piles of stones arranged in a row.  The `i`-th pile has `stones[i]` stones.

A move consists of merging **exactly** `K` **consecutive** piles into one pile, and the cost of this move is equal to the total number of stones in these `K` piles.

Find the minimum cost to merge all piles of stones into one pile.  If it is impossible, return -1.



**Note:**

- $1 \le \text{stones.length} \le 30$
- $2 \le K \le 30$
- $1 \le \text{stones[i]} \le 100$



## 题目解读

&emsp;每次合并连续的$K$堆石头，求将这些石头合并为$1$堆的最少的代价。

```java
class Solution {
    public int mergeStones(int[] stones, int K) {

    }
}
```

## 程序设计

* 参考社区思路，动态规划数组`dp(i,j,k)`表示区间`[i,j]`搬运为`k+1`堆的最小代价，考虑两种情况：
  * `dp(i,j,0)`：由于限定搬运连续的`K`个，如果`[i,j]`区间内能够搬运为`K`堆，则`dp(i,j,1) = dp(i,j,K-1) + cost(i~j)`，否则`[i,j]`无法搬运为一堆；
  * `dp(i,j,k)`，`k`取值在`(0,K)`：表示将区间内相邻的石头搬运为`k`堆的最小代价，不受`K`的限制，显然当`k`为区间长度时，`dp(i,j,k)=0`，不需搬运；否则尝试区间内的一点`j`，
    使得`[i,l]`搬运为与一堆，`[l+1,j]`搬运为`k-1`堆，尝试所有组合，选取最小值。
* 可看上述思路动态规划状态分两种，一种是`dp(i,j,0)`，受`K`的约束，一种是`dp(i,j,k)`。

```java
class Solution {
    public int mergeStones(int[] stones, int K) {
        if (stones == null) throw new IllegalArgumentException("invalid param");

        int N = stones.length;
        // 无法搬运为一堆
        if ((N - 1) % (K - 1) != 0) return -1;
        if (K > N) return 0;

        // 前缀和用于计算区间代价
        int[] preSum = new int[N + 1];
        for (int i = 1; i <= N; i++) {
            preSum[i] = preSum[i - 1] + stones[i - 1];
        }

        // 动态规划数组dp(i,j,k)表示将区间i~j搬运为k+1堆的最小代价
        int[][][] dp = new int[N][N][K];
        // 区间长度
        for (int len = 1; len <= N; len ++) {
            // 区间坐标
            for (int i = 0, j = i + len - 1; j < N; i++, j++) {
                // 区间和
                int cost = preSum[j + 1] - preSum[i];

                for (int k = 1; k < K; k++) {
                    dp[i][j][k] = Integer.MAX_VALUE;

                    for (int l = i; l < j; l++) {
                        if (dp[i][l][0] == Integer.MAX_VALUE || dp[l + 1][j][k - 1] == Integer.MAX_VALUE) continue;
                        // 尝试i~l划分为一堆，l~j划分为k堆的组合
                        dp[i][j][k] = Math.min(dp[i][j][k], dp[i][l][0] + dp[l + 1][j][k - 1]);
                    }
                }

                // 初始化，本来是一堆的代价为0
                if (i == j) dp[i][j][0] = 0;
                else dp[i][j][0] = Integer.MAX_VALUE;
                // 可搬运为一堆
                if (dp[i][j][K - 1] != Integer.MAX_VALUE) dp[i][j][0] = Math.min(dp[i][j][0], dp[i][j][K - 1] + cost);
            }
        }

        return dp[0][N - 1][0] == Integer.MAX_VALUE ? -1 : dp[0][N - 1][0];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^3K)$，空间复杂度为$O(N^2K)$。

执行用时：5ms，在所有java提交中击败了63.30%的用户。

内存消耗：39.4MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。参考社区思路，首先分析规律，当$(N - 1) \% (K - 1) = 0$时才可以堆为一堆，$N$为区间长度；动态规划`dp(i,j)`表示区间`[i,j]`合并到无法合并时的代价，这样可以尝试将左侧区间合并为一堆，右侧区间合并到最小，即`[i,k]`满足上述关系，尽力合并`[k+1,j]`，得到`dp(i,j) = min(dp(i,k) + dp(k+1,j))`，如果`[i,j]`区间可以合并为一堆，还需要加上搬运所有石头的开销。

```java
class Solution {
    public int mergeStones(int[] stones, int K) {
        if (stones == null) throw new IllegalArgumentException("invalid param");

        int N = stones.length;
        // 无法搬运为一堆
        if ((N - 1) % (K - 1) != 0) return -1;
        if (K > N) return 0;

        // 计算前缀和
        int[] preSum = new int[N + 1];
        for (int i = 1; i <= N; i++) {
            preSum[i] = preSum[i - 1] + stones[i - 1];
        }

        // 表示区间内合并到无法合并时的最小代价
        int[][] dp = new int[N][N];
        // 区间i~j，至少包含K个，否则无法合并，以左侧区间开始，右侧区间后移更新
        // 由于更新中需要用到后面区间的信息，故左侧区间需要从后往前更新
        for (int i = N - K; i >= 0; i--) {
            for (int j = i + K - 1; j < N; j++) {
                int min = Integer.MAX_VALUE;
                // 尝试将左侧堆叠为一堆（每次迭代加K-1保证可堆叠为一堆），右侧堆叠为无法合并堆，选择代价最小值
                for (int k = i; k < j; k += K - 1) {
                    min = Math.min(min, dp[i][k] + dp[k + 1][j]);
                }
                // i,j区间可合并为一堆，需要加上移动所需的所有石头数目
                if ((j - i) % (K - 1) == 0) {
                    min += preSum[j + 1] - preSum[i];
                }
                dp[i][j] = min;
            }
        }

        return dp[0][N - 1];
    }
}
```

&emsp;时间复杂度为$O(N^2K)$，空间复杂度为$O(N^2)$。

执行用时：2ms，在所有java提交中击败了99.08%的用户。

内存消耗：37.2MB，在所有java提交中击败了100.00%的用户。