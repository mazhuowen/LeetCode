[toc]

You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

**Note**:
You may assume that you have an infinite number of each kind of coin.



## 题目解读

&emsp;找零问题，币值自定义，需要动态规划解决。

```java
class Solution {
    public int coinChange(int[] coins, int amount) {

    }
}
```

## 程序设计

* 基本动态规划思路如下，时间复杂度为$O(N^2)$，会超出时间限制。

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        if (amount < 0) throw new IllegalArgumentException("invalid param");
        // 该题目特殊值，不能返回-1，返回0；
        if (amount == 0) return 0;

        int[] dp = new int[amount + 1];
        Arrays.fill(dp, -1);
        // 初始化
        for (int coin : coins) {
            if (coin <= amount) dp[coin] = 1;
        }

        for (int i = 1; i <= amount; i++) {
            for (int j = i - 1; j >= 1; j--) {
                // j和i-j无方案，跳过
                if (dp[j] == -1 || dp[i - j] == -1) continue;
                if (dp[i] == -1) dp[i] = dp[j] + dp[i - j];
                else dp[i] = Math.min(dp[i], dp[j] + dp[i - j]);
            }
        }

        return dp[amount];
    }
}
```

* 参考官方解题，根据`coins`数组遍历更新动态规划数组，而不是初始化到动态规划数组然后二次循环遍历。

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        if (amount < 0) throw new IllegalArgumentException("invalid param");

        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1);
        // 特殊值0，不能输出-1
        dp[0] = 0;

        for (int i = 1; i <= amount; i++) {
           for (int j = 0; j < coins.length; j++) {
               if (coins[j] <= i) {
                   dp[i] = Math.min(dp[i], dp[i - coins[j]] + 1);
               }
           }
        }

        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(N)$。其中$M$为硬币数。

执行用时：14ms，在所有java提交中击败了82.28%的用户。

内存消耗：39 MB，在所有java提交中击败了5.77%的用户。

## 官方解题

&emsp;参考官方解题。