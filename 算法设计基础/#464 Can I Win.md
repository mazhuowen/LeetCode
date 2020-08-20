[toc]

In the "100 game" two players take turns adding, to a running total, any integer from $1$ to $10$. The player who first causes the running total to **reach or exceed** 100 wins.

What if we change the game so that players **cannot** re-use integers?

For example, two players might take turns drawing from a common pool of numbers from $1$ to $15$ without replacement until they reach a total >= $100$.

Given two integers maxChoosableInteger and desiredTotal, return true if the first player to move can force a win, otherwise return false. Assume both players play **optimally**.

 

**Constraints:**

- $1 \le \text{maxChoosableInteger} \le 20$
- $0 \le \text{desiredTotal} \le 300$



## 题目解读

&emsp;给定牌$1 \sim \text{maxChoosableInteger}$，两个选手不放回抽取，谁先达到$\text{desiredTotal}$谁赢，假设两人都是最优选择，求先手是否能赢比赛。

```java
class Solution {
    public boolean canIWin(int maxChoosableInteger, int desiredTotal) {

    }
}
```

## 程序设计

* 需采用回溯暴力遍历尝试所有情况，由于会重复计算，引入额外的存储结构保存中间计算结果，加速计算。

```java
class Solution {
    // 状态数组，表示选择对应牌先手是否能赢，1能赢-1不能赢
    private int[] record;

    public boolean canIWin(int maxChoosableInteger, int desiredTotal) {
        // 先手选择大于等于desiredTotal的牌，必胜
        if (desiredTotal <= maxChoosableInteger) return true;
        // 牌的总和小于要求值，无法达到
        if (maxChoosableInteger * (maxChoosableInteger + 1) / 2 < desiredTotal) return false;
        // 牌的总和等于要求值，只有当牌是奇数个时，先手才会赢
        if (maxChoosableInteger * (maxChoosableInteger + 1) / 2 == desiredTotal) return maxChoosableInteger % 2 == 1;

        this.record = new int[1 << maxChoosableInteger];
        return backTracing(0, 0, desiredTotal, maxChoosableInteger);
    }

    private boolean backTracing(int stat, int curTotal, int desiredTotal, int maxChoosableInteger) {
        if (record[stat] != 0) return record[stat] == 1;

        if (desiredTotal == 0) return true;

        for (int i = 0; i < maxChoosableInteger; i++) {
            int mask = 1 << i;
            // 已抽取
            if ((stat & mask) == mask) continue;

            // 试探本轮抽取该牌
            int newTotal = curTotal + i + 1;
            // 赢的条件为当前综合大于等于要求值，或下一轮选手不能赢
            if (newTotal >= desiredTotal || !backTracing(stat | mask, newTotal, desiredTotal, maxChoosableInteger)) {
                record[stat] = 1;
                return true;
            }
        }
        record[stat] = -1;
        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N!)$，空间复杂度为$O(2^N)$。

执行用时：14ms，在所有java提交中击败了89.78%的用户。

内存消耗：43.7MB，在所有java提交中击败了59.39%的用户。

## 官方解题

&emsp;暂无，密切关注。