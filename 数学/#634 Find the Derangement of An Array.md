[toc]

In combinatorial mathematics, a derangement is a permutation of the elements of a set, such that no element appears in its original position.

There's originally an array consisting of $n$ integers from $1$ to $n$ in ascending order, you need to find the number of derangement it can generate.

Also, since the answer may be very large, you should return the output mod $10^9 + 7$.



**Note:**

* $n$ is in the range of $[1, 10^6]$.



## 题目解读

&emsp;错位排列指数字不在本位置的排列，求错位排列数。

```java
class Solution {
    public int findDerangement(int n) {

    }
}
```

## 程序设计

* 对于最简单的三个集合$\lvert A \cup B \cup C \rvert = \lvert A \rvert + \lvert B \rvert + \lvert C \rvert - \lvert A \cap B \rvert - \lvert A \cap C \rvert - \lvert B \cap C \rvert + \lvert A \cap B \cap C \rvert$，扩展到$n$个集合有：

$$
\left\vert \bigcup_{i = 1}^n A_i\right\vert = \sum_{i = 1}^n\left\vert A_i\right\vert - \sum_{1 \le i < j \le n}\left\vert A_i \cap A_j\right\vert + \sum_{i \le i < j < k \le n}\left\vert A_i \cap A_j \cap A_k\right\vert + \dots + (-1)^n\left\vert A_1 \cap \dots \cap A_n\right\vert\\
\left\vert\bigcap_{i = 1}^n\bar{A}_i\right\vert = \left\vert S - \bigcup A_i\right\vert
$$

* 在本题中，$A_i$表示第$i$个位置是$i$的排列数，带入得：

$$
\left\vert\bigcap_{i = 1}^n\bar{A}_i\right\vert = n! - n * (n - 1)! + \binom{n}{2}(n - 2)! - \dots + (-1)^n\binom{n}{n}(n - n)!\\
 = n! - \frac{n!}{1!} + \frac{n!}{2!} - \dots + (-1)^n\frac{n!}{n!}
$$

```java
class Solution {
    private final static int MOD = 1_000_000_007;

    public int findDerangement(int n) {
        long res = 0, val = 1;
        // 从公式右边开始叠加
        for (int i = n; i >= 0; i--) {
            res = (res + MOD + (i % 2 == 0 ? 1 : -1) * val) % MOD;
            // 前一项的值
            val = (val * i) % MOD;
        }
        return (int)res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：40ms，在所有java提交中击败了8.45%的用户。

内存消耗：35.2MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方除了上述思路还提供了动态规划的解法。`dp(n)`表示$n$个数错位的排列数目，则在原来的错位排列基础上有如下情况：

* 新增一个数，将该数字与前$n - 1$个数字替换，得到的仍然是错位数字，即`(n-1)dp(n-1)`；
* 前$n - 1$个数中有$n-2$个数是错位数字，新增一个数与前面的数交换位置得到新的错位数字，即`(n-1)dp(n-1)`。

```java
class Solution {
    private final static int MOD = 1_000_000_007;

    public int findDerangement(int n) {
        if (n == 0) return 1;

        long dp0 = 1, dp1 = 0;
        for (int i = 2; i <= n; i++) {
            long tmp = dp1;
            dp1 = ((i - 1) * (dp1 + dp0)) % MOD;
            dp0 = tmp;
        }
        return (int)dp1;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：24ms，在所有java提交中击败了100.00%的用户。

内存消耗：35MB，在所有java提交中击败了100.00%的用户。