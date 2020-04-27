[toc]

You are given coins of different denominations and a total amount of money. Write a function to compute the number of combinations that make up that amount. You may assume that you have infinite number of each kind of coin.



Note:

* $0 \le amount \le 5000$
* $1 \le coin \le 5000$
* the number of coins is less than $500$
* the answer is guaranteed to fit into signed 32-bit integer



## 题目解读

&emsp;给定不同面额的硬币和一个总金额。计算可以凑成总金额的硬币组合数。假设每一种面额的硬币有无限个。 

```java
class Solution {
    public int change(int amount, int[] coins) {
        
    }
}
```

## 程序设计

* 凑成总额的硬币组合不能有重复，如果按照常规以总额开始遍历，则不可避免的引入重复硬币组合；参考官方思路，采用币值遍历更新，避免重复。

```java
class Solution {
    public int change(int amount, int[] coins) {
        int[] dp = new int[amount + 1];
        dp[0] = 1;
        // 从币值开始遍历，避免造成重复，这样对于dp[i]当前的组合dp[i - coin]中没有coin，不会出现重复
        for (int coin : coins) {
            for (int i = coin; i <= amount; i++) {
                dp[i] += dp[i - coin];
            }
        }

        return dp[amount];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(KN)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了100.00%的用户。

内存消耗：37MB，在所有java提交中击败了8.33%的用户。

## 官方解题

&emsp;见上。