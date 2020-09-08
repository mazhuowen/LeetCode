[toc]

Given an array of scores that are non-negative integers. Player 1 picks one of the numbers from either end of the array followed by the player 2 and then player 1 and so on. Each time a player picks a number, that number will not be available for the next player. This continues until all the scores have been chosen. The player with the maximum score wins.

Given an array of scores, predict whether player 1 is the winner. You can assume each player plays to maximize his score.



**Constraints**:

* $1 \le \text{length of the array} \le 20$.
* Any scores in the given array are non-negative integers and will not exceed 10,000,000.
  If the scores of both players are equal, then player 1 is still the winner.



## 题目解读

&emsp;给定一个表示分数的非负整数数组。玩家1从数组任意一端拿取一个分数，随后玩家2继续从剩余数组任意一端拿取分数，然后玩家1拿，……。每次一个玩家只能拿取一个分数，分数被拿取之后不再可取。直到没有剩余分数可取时游戏结束。最终获得分数总和最多的玩家获胜。预测玩家1是否胜利。

```java
class Solution {
    public boolean PredictTheWinner(int[] nums) {
        
    }
}
```

## 程序设计

* 两个玩家只能从数组两端选择，最终获得分数最大的人获胜。可用动态规划数组`dp(i,j)`表示数组区间$i \sim j$中两个选手得分的最大差值，则`dp(i,j) = max(nums[i] - dp(i+1,j), nums[j] - dp(i,j-1))`。

```java
class Solution {
    public boolean PredictTheWinner(int[] nums) {
        if (nums == null || nums.length == 0) return true;

        int n = nums.length;
        int[][] dp = new int[n][n];
        // 初始化只有一个元素时的最大差
        for (int i = 0; i < n; i++) dp[i][i] = nums[i];
        
        for (int i = n - 2; i >= 0; i--) {
            for (int j = i + 1; j < n; j++) {
                // 先手选择i或j的最大分差为当前得分加上后手的最大分差的负数
                dp[i][j] = Math.max(nums[j] - dp[i][j - 1], nums[i] - dp[i + 1][j]);
            }
        }

        return dp[0][n - 1] >= 0;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.7MB，在所有java提交中击败了99.10%的用户。

## 官方解题

&emsp;上述思路参考官方。官方还提供了递归计算的思路，时间复杂度为$O(2^N)$。