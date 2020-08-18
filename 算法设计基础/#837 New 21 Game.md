[toc]

Alice plays the following game, loosely based on the card game "21".

Alice starts with $0$ points, and draws numbers while she has less than $K$ points.  During each draw, she gains an integer number of points randomly from the range $[1, W]$, where $W$ is an integer.  Each draw is independent and the outcomes have equal probabilities.

Alice stops drawing numbers when she gets K or more points.  What is the probability that she has $N$ or less points?



**Note**:

* $0 \le K \le N \le 10000$
* $1 \le W \le 10000$
* Answers will be accepted as correct if they are within $10^{-5}$ of the correct answer.
* The judging time limit has been reduced for this question.



## 题目解读

&emsp;当累积分值小于$K$时，在$[1,W]$的牌中抽取一张并加入累积分数，最后求抽取结束后累积分值小于等于$N$的概率。

```java
class Solution {
    public double new21Game(int N, int K, int W) {

    }
}
```

## 程序设计

* 首先想到的是使用动态规划数组`dp(i)`表示积分为$i$的概率；如果$i < K$，则下次抽牌后的累积分数为$i + 1 \sim i + W$，概率在原先的基础上乘以$\frac{1}{W}$并叠加。该思路时间复杂度为$O(KW)$，会超时。

```java
class Solution {
    public double new21Game(int N, int K, int W) {
        if (K < 0 || N < K) throw new IllegalArgumentException("invalid param");
        if (K == 0) return 1.0D;

        // 累积分数对应的概率
        double[] dp = new double[K + W];
        dp[0] = 1.0D;

        for (int i = 0; i < K; i++) {
            for (int j = 1; j <= W; j++) {
                dp[i + j] += dp[i] / W;
            }
        }

        // 统计累积分数在K~N间的概率
        double res = 0;
        for (int i = K; i <= N; i++) {
            res += dp[i];
        }
        return res;
    }
}
```

* 参考官方思路，正向推导是利用`dp(i)`更新`dp(i+1)~dp(i+W)`，可以反向思维逆向推导，即初始化`K~N`概率为$1$，这样可由`dp(i+1)~dp(i+W)`推导`dp(i)`，可得出$dp(i) = \frac{dp(i + 1) + \dots + dp(i + W)}{W}$。
* 由于内层循环超时，探索前后关系发现$dp(i + 1) - dp(i) = \frac{dp(i + W + 1) - dp(i + 1)}{W}$，这样可由更新过的值得到当前值，还无需遍历。
* 对于$dp(K - 1)$无法利用上式更新，根据定义$dp(K - 1) = \frac{dp(K) + \dots + dp(K + W - 1)}{W} = \frac{\min(K + W - 1,N) - K + 1}{W}$。

```java
class Solution {
    public double new21Game(int N, int K, int W) {
        if (K < 0 || N < K) throw new IllegalArgumentException("invalid param");
        if (K == 0) return 1.0D;

        // 累积分数对应的概率
        double[] dp = new double[K + W];
        for (int i = K; i < K + W && i <= N; i++) dp[i] = 1.0D;
        // 特殊值K-1计算
        dp[K - 1] = (double)Math.min(N - K + 1, W) / W;

        // 从后往前更新
        for (int i = K - 2; i >= 0; i--) {
            dp[i] = dp[i + 1] - (dp[i + W + 1] - dp[i + 1]) / W;
        }
        return dp[0];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(K + W)$，空间复杂度为$O(K + W)$。

执行用时：4ms，在所有java提交中击败了93.69%的用户。

内存消耗：39.2MB，在所有java提交中击败了59.61%的用户。

## 官方解题

&emsp;上述思路参考官方解题。