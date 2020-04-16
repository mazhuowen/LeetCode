[toc]

A die simulator generates a random number from 1 to 6 for each roll. You introduced a constraint to the generator such that it cannot roll the number `i` more than `rollMax[i]` (1-indexed) **consecutive** times. 

Given an array of integers `rollMax` and an integer `n`, return the number of distinct sequences that can be obtained with exact `n` rolls.

Two sequences are considered different if at least one element differs from each other. Since the answer may be too large, return it modulo $10^9 + 7$.



**Constraints:**

- $1 \le n \le 5000$
- $\text{rollMax.length} == 6$
- $1 \le \text{rollMax[i]} \le 15$



## 题目解读

&emsp;给定$n$，求投掷$n$次骰子可能的序列，其中给出`rollMax`规定同一个数最大连续出现的长度。

```java
class Solution {
    public int dieSimulator(int n, int[] rollMax) {

    }
}
```

## 程序设计

* 最基本的思路是回溯，根据前一个数字及其计数来判断下一次尝试。最坏情况下时间复杂度接近$O(6^N)$，会超时。

```java
class Solution {
    int count = 0;

    public int dieSimulator(int n, int[] rollMax) {
        if (n <= 0) return 0;
        dieSimulator(0, 0, n, rollMax);
        return count;
    }

    private void dieSimulator(int pre, int preCount, int n, int[] rollMax) {
        // 找到一组可行解
        if (n == 0) {
            count++;
            return;
        }
        // 模拟骰子
        for (int i = 1; i <= 6; i++) {
            // 尝试跟之前一致的数
            if (i == pre) {
                if (preCount + 1 <= rollMax[i - 1]) dieSimulator(pre, preCount + 1, n - 1, rollMax);
            } 
            // 尝试新的数
            else {
                dieSimulator(i, 1, n - 1, rollMax);
            }
        }
    }
}
```

* 事实上，上述方法会重复尝试序列。可设定动态规划数组。

```java
class Solution {
    int mod = 1_000_000_007;

    public int dieSimulator(int n, int[] rollMax) {
        // 表示i个序列以j结尾且计数为k的所有序列数目
        int[][][] dp = new int[n][6][15];
        // 初始化一个序列的情况
        for (int i = 0; i < 6; i++) {
            dp[0][i][0] = 1;
        }
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < 6; j++) {
                for (int k = 0; k < 15; k++) {
                    // 前一序列以j结尾且计数为k个
                    int num = dp[i - 1][j][k];
                    // 加速，提前终止
                    if (num == 0) continue;
                    for (int l = 0; l < 6; l++) {
                        // 若拼接相同值j，判断计数并更新
                        if (l == j) {
                            if (k + 2 <= rollMax[l]) dp[i][j][k + 1] = (dp[i][j][k + 1] + num) % mod;
                        }
                        // 拼接不同值l，则计数重置为1
                        else {
                            dp[i][l][0] = (dp[i][l][0] + num) % mod;
                        }
                    }
                }
            }
        }
        int count = 0;
        // 遍历序列为n的所有结尾的种数，计算求和
        for (int i = 0; i < 6; i++) {
            for (int j = 0; j < 15; j++) {
                count += dp[n - 1][i][j];
                count %= mod;
            }
        }
        return count;
    }
}
```

* 仔细观察上述思路，动态规划只需要上一个序列，不需要存储所有序列的值，可将三维数组优化为二维。

```java
class Solution {
    int mod = 1_000_000_007;

    public int dieSimulator(int n, int[] rollMax) {
        // 表示前一个序列以i结尾且计数为j的所有序列数目
        int[][] dp = new int[6][15];
        // 初始化一个序列的情况
        for (int i = 0; i < 6; i++) {
            dp[i][0] = 1;
        }
        for (int i = 1; i < n; i++) {
            int[][] temp = new int[6][15];
            for (int j = 0; j < 6; j++) {
                for (int k = 0; k < 15; k++) {
                    // 前一序列以j结尾且计数为k个
                    int num = dp[j][k];
                    // 加速，提前终止
                    if (num == 0) continue;
                    for (int l = 0; l < 6; l++) {
                        // 若拼接相同值j，判断计数并更新
                        if (l == j) {
                            if (k + 2 <= rollMax[l]) temp[j][k + 1] = (temp[j][k + 1] + num) % mod;
                        }
                        // 拼接不同值l，则计数重置为1
                        else {
                            temp[l][0] = (temp[l][0] + num) % mod;
                        }
                    }
                }
            }
            dp = temp;
        }
        int count = 0;
        // 遍历序列为n的所有结尾的种数，计算求和
        for (int i = 0; i < 6; i++) {
            for (int j = 0; j < 15; j++) {
                count += dp[i][j];
                count %= mod;
            }
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：63ms，在所有java提交中击败了46.30%的用户。

内存消耗：40.5MB，在所有java提交中击败了100.00%的用户。

&emsp;优化后时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：37ms，在所有java提交中击败了76.85%的用户。

内存消耗：39.4 MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区动态规划是另一种思路，即当前序列数目为所有组合数减去不符合要求的序列数目。

```java
class Solution {
    int mod = 1_000_000_007;

    public int dieSimulator(int n, int[] rollMax) {
        // 表示第i个序列以j结尾的序列数目，其中索引6表示当前序列所有的数目
        int[][] dp = new int[n + 1][7];
        // 起始为1
        dp[0][6] = 1;

        for (int i = 1; i <= n; i++) {
            for (int j = 0; j < 6; j++) {
                // 先取前一序列的总数
                dp[i][j] = dp[i - 1][6];
                // 减去超过最大连续值结尾的数目
                if (i > rollMax[j]) {
                    // 减去rollMax[j]-1前的数目，即拼接超过rollMax[j]个j的数目
                    // 减去dp[i - rollMax[j] - 1][j]是因为这部分值已经在i-1时减去了，不需要重复减
                    int reduceIdx = i - rollMax[j] - 1;
                    int reduce = (dp[reduceIdx][6] + mod - dp[reduceIdx][j]) % mod;
                    dp[i][j] = (dp[i][j] + mod - reduce) % mod;
                }

                // 计入总数
                dp[i][6] += dp[i][j];
                dp[i][6] %= mod;
            }
        }
        return dp[n][6];
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：8ms，在所有java提交中击败了87.96%的用户。

内存消耗：39MB，在所有java提交中击败了100.00%的用户。