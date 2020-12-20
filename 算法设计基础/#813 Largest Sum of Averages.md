[toc]

We partition a row of numbers `A` into at most `K` adjacent (non-empty) groups, then our score is the sum of the average of each group. What is the largest score we can achieve?

Note that our partition must use every number in A, and that scores are not necessarily integers.



```
Example:
Input: 
A = [9,1,2,3,9]
K = 3
Output: 20
Explanation: 
The best choice is to partition A into [9], [1, 2, 3], [9]. The answer is 9 + (1 + 2 + 3) / 3 + 9 = 20.
We could have also partitioned A into [9, 1], [2], [3, 9], for example.
That partition would lead to a score of 5 + 2 + 6 = 13, which is worse.
```



**Note**:

* $1 \le \text{A.length} \le 100$.
* $1 \le \text{A[i]} \le 10000$.
* $1 \le K \le \text{A.length}$.
* Answers within $10^{-6}$ of the correct answer will be accepted as correct.



## 题目解读

&emsp;将数组切分为连续的$K$段，求每段平均值之和的最大值。

```java
class Solution {
    public double largestSumOfAverages(int[] A, int K) {

    }
}
```

## 程序设计

* 使用`dp(i,j)`表示$0 \sim i$的数组分为$j$段的最大平均值之和。

```java
class Solution {
    public double largestSumOfAverages(int[] A, int K) {
        if (A == null || A.length < K) throw new IllegalArgumentException("invalid param");
        // 计算前缀和
        double[] preSum = new double[A.length + 1];
        for (int i = 0; i < A.length; i++) preSum[i + 1] = preSum[i] + A[i];
        // 表示0~i分为j段的最大平均值之和
        double[][] dp = new double[A.length][K + 1];
        // 初始化
        for (int i = 0; i < A.length; i++) dp[i][1] = preSum[i + 1] / (i + 1);
        for (int i = 0; i < A.length; i++) {
            for (int j = 2; j <= K && j <= i + 1; j++) {
                // [k,i]作为一段
                for (int k = j - 1; k <= i; k++) {
                    dp[i][j] = Math.max(dp[i][j], dp[k - 1][j - 1] + (preSum[i + 1] - preSum[k]) / (i - k + 1));
                }
            }
        }
        return dp[A.length - 1][K];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2K)$，空间复杂度为$O(NK)$。

执行用时：10 ms, 在所有 Java 提交中击败了65.90%的用户。

内存消耗：36.4 MB, 在所有 Java 提交中击败了72.85%的用户。

## 官方解题

&emsp;官方思路类似，进一步优化空间复杂度，调换内层喜欢，压缩动态规划数组。

```java
class Solution {
    public double largestSumOfAverages(int[] A, int K) {
        if (A == null || A.length < K) throw new IllegalArgumentException("invalid param");
        // 计算前缀和
        double[] preSum = new double[A.length + 1];
        for (int i = 0; i < A.length; i++) preSum[i + 1] = preSum[i] + A[i];
        // 表示0~i分为j段的最大平均值之和
        double[] dp = new double[A.length];
        for (int i = 0; i < A.length; i++) dp[i] = preSum[i + 1] / (i + 1);
        for (int k = 2; k <= K; k++) {
            for (int i = A.length - 1; i >= 0; i--) {
                for (int j = k - 1; j <= i; j++) {
                    dp[i] = Math.max(dp[i], dp[j - 1] + (preSum[i + 1] - preSum[j]) / (i - j + 1));
                }
            }
        }
        return dp[A.length - 1];
    }
}
```

&emsp;时间复杂度为$O(N^2K)$，空间复杂度为$O(N)$。

执行用时：8 ms, 在所有 Java 提交中击败了88.52%的用户。

内存消耗：36.2 MB, 在所有 Java 提交中击败了84.44%的用户。