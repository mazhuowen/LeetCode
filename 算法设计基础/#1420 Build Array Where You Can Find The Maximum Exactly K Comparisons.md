[toc]

Given three integers $n$, $m$ and $k$. Consider the following algorithm to find the maximum element of an array of positive integers:

<img src="..\images\#1420.png" style="zoom: 80%;" />


You should build the array arr which has the following properties:

* `arr` has exactly $n$ integers.
* $1 \le \text{arr[i]} \le m$ where ($0 \le i < n$).
* After applying the mentioned algorithm to `arr`, the value `search_cost` is equal to $k$.

Return the number of ways to build the array `arr` under the mentioned conditions. As the answer may grow large, the answer **must be** computed modulo $10^9 + 7$.



**Constraints:**

- $1 \le n \le 50$
- $1 \le m \le 100$
- $0 \le k \le n$



## 题目解读

&emsp;给定长度$n$，最大数字$m$，搜索消耗$k$，求满足条件的数组数目。

```java
class Solution {
    public int numOfArrays(int n, int m, int k) {

    }
}
```

## 程序设计

* 对于题目给定搜索最大值的代价$k$，可看作是$k$个呈严格递增的山顶；如果新加入的山顶高于之前的最高山顶，则最大代价增加；如果小于等于，则不变；
* 有了上述分析，可用`dp(i,j,k)`表示前$i$个序列，最大值为$j$，搜索代价为$k$，则对于$i + 1$个序列：
  * 插入$i + 1$位置的值是最大值$j^\prime$，大于之前所有元素，则此时代价值增加，`dp(i+1,j',k+1) = sum(dp(i,j,k))`，其中$1 \le j < j^\prime$；
  * 插入$i + 1$位置的值小于等于之前元素最大值，即$j^\prime \le j$，代价不变，由于$j^\prime$有$j$个选择，故`dp(i + 1,j',k) = j' * dp(i,j',k)`。
* 初始化时，对于`j=1`，即所有元素及最大值为$1$，此时`dp(i,1,1)=1`，`dp(i,1,k)=0`；对于序列只有一个数的情况，`dp(1,j,1)=1`，`dp(1,j,k)=0`。

```java
class Solution {
    private final static int MOD = 1_000_000_007;

    public int numOfArrays(int n, int m, int k) {
        // dp(i,j,k)表示i长序列包含最大值j的数组有k个搜索消耗的数目
        long[][][] dp = new long[n + 1][m + 1][k + 1];
        // 初始化，只包含1的序列搜索代价为1的数目为1
        for (int i = 1; i <= n; i++) dp[i][1][1] = 1;
        // 序列长度为1最大值为j+1的搜索代价数目为1
        for (int j = 1; j <= m; j++) dp[1][j][1] = 1;

        for (int i = 2; i <= n; i++) {
            // 注意j从2开始，j=1已初始化
            for (int j = 2; j <= m; j++) {
                for (int l = 1; l <= k && l <= i; l++) {
                    // 末尾插入最大值
                    for (int h = 1; h < j; h++) dp[i][j][l] = (dp[i][j][l] + dp[i - 1][h][l - 1]) % MOD;
                    // 末尾插入其他值
                    dp[i][j][l] = (dp[i][j][l] + j * dp[i - 1][j][l]) % MOD;
                }
            }
        }

        long res = 0;
        for (int j = 1; j <= m; j++) {
            res = (res + dp[n][j][k]) % MOD;
        }
        return (int)res;
    }
}
```

* 优化空间和时间性能，对于空间复杂度，由于每次依赖前一时间，可以简化数组；对于时间复杂度，第四个循环可以和第二个循环合并，再调整循环顺序即可。

```java
class Solution {
    private final static int MOD = 1_000_000_007;

    public int numOfArrays(int n, int m, int k) {
        long[][] dp = new long[m + 1][k + 1];
        // 初始化序列为1，最大数字为j，k为1只有一种方案
        for (int j = 1; j <= m; j++) dp[j][1] = 1;

        for (int i = 2; i <= n; i++) {
            long[][] tmp = new long[m + 1][k + 1];
            // 初始化序列为i，只有一个数字1，方案只有一种
            tmp[1][1] = 1;
            for (int l = 1; l <= k && l <= i; l++) {
                long sum = 0;
                for (int j = 2; j <= m; j++) {
                    sum = (sum + dp[j - 1][l - 1]) % MOD;
                    tmp[j][l] = (tmp[j][l] + sum + j * dp[j][l]) % MOD;
                }
            }
            dp = tmp;
        }

        long res = 0;
        for (int j = 1; j <= m; j++) {
            res = (res + dp[j][k]) % MOD;
        }
        return (int)res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM^2K)$，空间复杂度为$O(NMK)$。

执行用时：113ms，在所有java提交中击败了30.84%的用户。

内存消耗：39.9MB，在所有java提交中击败了43.48%的用户。

&emsp;优化后时间复杂度为$O(NMK)$，空间复杂度为$O(MK)$。

执行用时：10ms，在所有java提交中击败了98.13%的用户。

内存消耗：38.7MB，在所有java提交中击败了71.74%的用户。

## 官方解题

&emsp;暂无，密切关注。