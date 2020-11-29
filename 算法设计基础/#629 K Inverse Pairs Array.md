[toc]

Given two integers `n` and `k`, find how many different arrays consist of numbers from `1` to `n` such that there are exactly `k` inverse pairs.

We define an inverse pair as following: For `ith` and `jth` element in the array, if `i < j` and `a[i] > a[j]` then it's an inverse pair; Otherwise, it's not.

Since the answer may be very large, the answer should be modulo $10^9 + 7$.



**Note:**

The integer `n` is in the range `[1, 1000]` and `k` is in the range `[0, 1000]`.



## 题目解读

&emsp;给定数字$n$，求$1 \sim n$序列排列组合中有$k$个逆序对的排列数目。

```java
class Solution {
    public int kInversePairs(int n, int k) {

    }
}
```

## 程序设计

* 假设`dp(i,j)`表示前$i + 1$个数字组成的序列中有$k$个逆序对的排列数目，现在考虑$i + ２$个数的序列，可将$i + 1$插入到$i + 1$的序列中，对于`dp(i,j)`，只需要将$i + 1$插入到最后，形成`dp(i+1,j)`；对于`dp(i,j - 1)`，将$i + 1$插入到倒数第二个位置，则构成`dp(i+1,j)`，以此类推，对于`dp(i,j-i)`（如果存在的话），只需要将$i + 1$插入到序列开头，形成`dp(i+1,j)`。
* 这样得到方程$dp(i,j) = \sum_{k = 0}^{i - 1}dp(i - 1, j - k) = \sum_{k = 0}^{i - 1}dp(i - 1, j - k - 1) + dp(i - 1, j) - dp(i - 1, j - i)$，从而得到$dp(i,j) = d(i, j - 1) + d(i - 1,j) - d(i - 1,j - i)$。当$k = 0$时，所有$n \ge 2$的$dp(n,0) = 1$，为顺序序列，此外由于$dp(1,1) = 0$，为了一般化，也初始化$dp(0,0)$、$dp(1,0)$为1，得到动态规划形式如下：

```java
class Solution {
    int mod = 1_000_000_007;

    public int kInversePairs(int n, int k) {
        // 动态规划数组，dp(i,j)表示由前i个数字（包括i）构成的序列中有j个逆序对
        int[][] dp = new int[n + 1][k + 1];
        // 逆序对为0则组合数为1，为顺序
        for (int i = 0; i <= n; i++) {
            dp[i][0] = 1;
        }
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= k; j++) {
                // dp[i][j] = dp[i - 1][j] + dp[i][j - 1] - dp[i - 1][j - i] 需判断dp[i - 1][j - i]的存在性
                dp[i][j] = (dp[i - 1][j] + dp[i][j - 1]) % mod;
                if (j >= i) {
                    // 需注意模差值为负的情况
                    dp[i][j] = (dp[i][j] + mod - dp[i - 1][j - i]) % mod;
                }
            }
        }
        return dp[n][k];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NK)$，空间复杂度为$O(NK)$。

执行用时：26ms，在所有java提交中击败了55.56%的用户。

内存消耗：40.1MB，在所有java提交中击败了33.33%的用户。

## 官方解题

&emsp;同上。