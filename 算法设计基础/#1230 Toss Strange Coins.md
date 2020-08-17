[toc]

You have some coins.  The `i`-th coin has a probability `prob[i]` of facing heads when tossed.

Return the probability that the number of coins facing heads equals `target` if you toss every coin exactly once.



**Constraints**:

* $1 \le \text{prob.length} \le 1000$
* $0 \le \text{prob[i]} \le 1$
* $0 \le \text{target} \le \text{prob.length}$
* Answers will be accepted as correct if they are within $10^{-5}$ of the correct answer.



## 题目解读

&emsp;求`target`个硬币正面朝上的概率。

```java
class Solution {
    public double probabilityOfHeads(double[] prob, int target) {

    }
}
```

## 程序设计

* 可用动态规划数组`dp(i,j)`表示$0 \sim i$个序列有$j$个朝上的概率。

```java
class Solution {
    public double probabilityOfHeads(double[] prob, int target) {
        int n = prob.length;
        double[][] dp = new double[n + 1][target + 1];
        dp[0][0] = 1;

        for (int i = 1; i <= n; i++) {
            for (int j = 0; j <= target; j++) {
                // 当前硬币反面朝上
                dp[i][j] = dp[i - 1][j] * (1 - prob[i - 1]);
                // 当前硬币正面朝上
                if (j >= 1) dp[i][j] += dp[i - 1][j - 1] * prob[i - 1];
            }
        }

        return dp[n][target];
    }
}
```

* 优化空间复杂度：

```java
class Solution {
    public double probabilityOfHeads(double[] prob, int target) {
        int n = prob.length;
        double[] dp = new double[target + 1];
        dp[0] = 1;

        for (int i = 1; i <= n; i++) {
            for (int j = Math.min(target, i); j >= 0; j--) {
                // 当前硬币反面朝上
                dp[j] = dp[j] * (1 - prob[i - 1]);
                // 当前硬币正面朝上
                if (j >= 1) dp[j] += dp[j - 1] * prob[i - 1];
            }
        }

        return dp[target];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(NM)$。

执行用时：23ms，在所有java提交中击败了50.56%的用户。

内存消耗：49.2MB，在所有java提交中击败了84.62%的用户。

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(M)$。

执行用时：13ms，在所有java提交中击败了96.63%的用户。

内存消耗：40.7MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。