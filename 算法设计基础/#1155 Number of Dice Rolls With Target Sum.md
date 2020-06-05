[toc]

You have `d` dice, and each die has `f` faces numbered `1`, `2`, ..., `f`.

Return the number of possible ways (out of $f^d$ total ways) **modulo** $10^9 + 7$ to roll the dice so the sum of the face up numbers equals `target`.



**Constraints:**

- $1 \le d, f \le 30$
- $1 \le \text{target} \le 1000$



## 题目解读

&emsp;给定`d`个`f`面的筛子，求`d`个筛子朝上面的和等于目标值的排列数。

```java
class Solution {
    public int numRollsToTarget(int d, int f, int target) {

    }
}
```

## 程序设计

* 最基本的思路是暴力回溯，时间复杂度为$O(f^d)$，会超时。可引入额外的存储空间存储中间值。

```java
class Solution {
    private static final int MOD = 1_000_000_007;
    Integer[][][] record;

    public int numRollsToTarget(int d, int f, int target) {
        if (record == null) record = new Integer[d + 1][f + 1][target + 1];

        if (d == 0 && target == 0) return 1;
        if (d == 0 || target < 0) return 0;
        if (record[d][f][target] != null) return record[d][f][target];

        int count = 0;
        for (int i = f; i >= 1; i--) {
            // 筛子数量减少，目标和减少，此处是排列数目，如果是组合数目，筛子只能尝试i及以下的值（去重）
            count += numRollsToTarget(d - 1, f, target - i);
            count %= MOD;
        }
        record[d][f][target] = count;
        return count;
    }
}
```

* 有了上述思路，可转化为动态规划，`dp(i,j)`表示前`i`个筛子组合值为`j`的数目；则`dp(i,j)=dp(i-1,j-k)`，其中`k`为筛子的值。

```java
class Solution {
    private static final int MOD = 1_000_000_007;

    public int numRollsToTarget(int d, int f, int target) {
        int[][] dp = new int[d + 1][target + 1];
        dp[0][0] = 1;

        for (int i = 1; i <= d; i++) {
            for (int j = 1; j <= f; j++) {
                for (int k = j; k <= target; k++) {
                    dp[i][k] += dp[i - 1][k - j];
                    dp[i][k] %= MOD;
                }
            }
        }

        return dp[d][target];
    }
}
```

* 由于只依赖前一个的状态，压缩状态。

```java
class Solution {
    private static final int MOD = 1_000_000_007;

    public int numRollsToTarget(int d, int f, int target) {
        int[] pre = new int[target + 1];
        pre[0] = 1;

        for (int i = 1; i <= d; i++) {
            int[] cur = new int[target + 1];
            for (int j = 1; j <= f; j++) {
                for (int k = j; k <= target; k++) {
                    cur[k] += pre[k - j];
                    cur[k] %= MOD;
                }
            }
            pre = cur;
        }

        return pre[target];
    }
}
```

## 性能分析

&emsp;记忆化回溯时间复杂度为$O(d * f * target)$，空间复杂度为$O(d * f * target)$。

执行用时：19ms，在所有java提交中击败了68.47%的用户。

内存消耗：42MB，在所有java提交中击败了100.00%的用户。

&emsp;动态规划时间复杂度为$O(d * f * target)$，空间复杂度为$O(d * target)$。

执行用时：25ms，在所有java提交中击败了61.46%的用户。

内存消耗：38.5MB，在所有java提交中击败了100.00%的用户。

&emsp;空间优化动态规划时间复杂度为$O(d * f * target)$，空间复杂度为$O(target)$。

执行用时：22ms，在所有java提交中击败了64.97%的用户。

内存消耗：38.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。参考社区思路，在循环`target`时，加入界限判断，大大减少无用循环。

```java
class Solution {
    private static final int MOD = 1_000_000_007;

    public int numRollsToTarget(int d, int f, int target) {
        int[] pre = new int[target + 1];
        pre[0] = 1;

        for (int i = 1; i <= d; i++) {
            int[] cur = new int[target + 1];
            for (int j = 1; j <= f; j++) {
                int sum = Math.min(target, i * f);
                for (int k = j; k <= sum; k++) {
                    cur[k] += pre[k - j];
                    cur[k] %= MOD;
                }
            }
            pre = cur;
        }

        return pre[target];
    }
}
```

执行用时：9ms，在所有java提交中击败了92.36%的用户。

内存消耗：38.7MB，在所有java提交中击败了100.00%的用户。