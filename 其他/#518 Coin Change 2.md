[toc]

You are given coins of different denominations and a total amount of money. Write a function to compute the number of combinations that make up that amount. You may assume that you have infinite number of each kind of coin.



**Note**:

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

* 粗略分析似乎是完全背包问题的变形，每种币值可以有无限件，需要填满总额。现在问题变为这样得到的组合是否不是重复的；由于题目保证给定的币值不重复，这样`dp(i,j)`表示前$i$个币值可组成的总额$j$的组合数目，对于前$i + 1$个币值，选择加入或不加入，如果加入，由于币值不同，不会造成重复。
* 首先问题为正好填满，故初始化需要将`dp(i,0)`初始化为与`dp(i,j)`不同的值；此处求解组合数，初始化为1，其他为0；其次是完全背包问题，由于求解的是组合数而非最少或最多零钱，故状态转移方程需要叠加而不是取最大最小。

```java
class Solution {
    public int change(int amount, int[] coins) {
        // 动态规划数组，表示前i个物品可恰好构成j的总额的组合数
        int[][] dp = new int[coins.length + 1][amount + 1];
        // 问题为正好填满，只初始化金额为0的组合数为1，其他为0
        for (int i = 0; i <= coins.length; i++) dp[i][0] = 1;

        for (int i = 1; i <= coins.length; i++) {
            for (int j = amount; j >= 0; j--) {
                dp[i][j] = dp[i - 1][j];
                for (int k = 1; k * coins[i - 1] <= j; k++) {
                    // 采用叠加
                    dp[i][j] += dp[i - 1][j - k * coins[i - 1]];
                }
            }
        }

        return dp[coins.length][amount];
    }
}
```

* 由于本体是组合问题，使用上述原始背包思路可以得到正确答案；利用排列组合问题的变形优化动态规划数组为一维，由于本题实际上是组合问题，不能调换内外层循环：

```java
class Solution {
    public int change(int amount, int[] coins) {
        int[] dp = new int[amount + 1];
        dp[0] = 1;
        for (int coin : coins) {
            // 顺序遍历，模拟k的叠加过程
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

&emsp;同上。