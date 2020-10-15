[toc]

We are playing the Guessing Game. The game will work as follows:

* I pick a number between $1$ and $n$.
* You guess a number.
* If you guess the right number, **you win the game**.
* If you guess the wrong number, then I will tell you whether the number I picked is **higher or lower**, and you will continue guessing.
* Every time you guess a wrong number x, you will pay x dollars. If you run out of money, **you lose the game**.

Given a particular $n$, return the minimum amount of money you need to **guarantee a win regardless of what number I pick**.



**Constraints:**

- $1 \le n \le 200$



## 题目解读

&emsp;求可猜得正确数字的最少金额。

```java
class Solution {
    public int getMoneyAmount(int n) {

    }
}
```

## 程序设计

* 实际就是最小最大问题，即在区间猜一个数的最大代价中的最小值。

```java
class Solution {
    int[][] record;

    public int getMoneyAmount(int n) {
        record = new int[n + 1][n + 1];
        return getMoneyAmount(1, n);
    }

    // 返回最小的最多所需金额
    private int getMoneyAmount(int left, int right) {
        if (left >= right) return 0;
        if (record[left][right] != 0) return record[left][right];
        int res = Integer.MAX_VALUE;
        for (int i = left; i <= right; i++) {
            // 最大代价中的最小值
            res = Math.min(res, i + Math.max(getMoneyAmount(left, i - 1), getMoneyAmount(i + 1, right)));
        }
        return record[left][right] = res;
    }
}
```

* 可使用动态规划。

```java
class Solution {
    public int getMoneyAmount(int n) {
        int[][] dp = new int[n + 2][n + 2];

        for (int len = 2; len <= n; len++) {
            for (int start = 1, end = start + len - 1; end <= n; start++, end++) {
                int res = Integer.MAX_VALUE;
                // 选择猜测数字
                for (int split = start; split <= end; split++) {
                    res = Math.min(res, split + Math.max(dp[start][split - 1], dp[split + 1][end]));
                }
                dp[start][end] = res;
            }
        }
        return dp[1][n];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^3)$，空间复杂度为$O(N^2)$。

执行用时：22ms，在所有java提交中击败了21.67%的用户。

内存消耗：38.1MB，在所有java提交中击败了7.16%的用户。

## 官方解题

&emsp;官方思路类似，对于猜测值得区间做了优化，不是从$(i,j)$遍历，而是从$(\frac{i + j}{2},j)$遍历。

```java
class Solution {
    public int getMoneyAmount(int n) {
        int[][] dp = new int[n + 2][n + 2];

        for (int len = 2; len <= n; len++) {
            for (int start = 1, end = start + len - 1; end <= n; start++, end++) {
                int res = Integer.MAX_VALUE;
                for (int split = (start + end) / 2; split < end; split++) {
                    res = Math.min(res, split + Math.max(dp[start][split - 1], dp[split + 1][end]));
                }
                dp[start][end] = res;
            }
        }
        return dp[1][n];
    }
}
```