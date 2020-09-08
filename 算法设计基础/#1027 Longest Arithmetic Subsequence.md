[toc]

Given an array `A` of integers, return the **length** of the longest arithmetic subsequence in `A`.

Recall that a subsequence of A is a list $A[i_1], A[i_2], \dots, A[i_k]$ with $0 \le i_1 < i_2 < ... < i_k \le \text{A.length} - 1$, and that a sequence `B` is arithmetic if $B[i+1] - B[i]$ are all the same value (for $0 \le i < \text{B.length} - 1$).



**Constraints:**

- $2 \le \text{A.length} \le 1000$
- $0 \le \text{A[i]} \le 500$



## 题目解读

&emsp;给定数组，返回最长等差子序列。

```java
class Solution {
    public int longestArithSeqLength(int[] A) {
        
    }
}
```

## 程序设计

* 动态规划数组`dp(i,j)`表示以$i$、$j$两个索引数字结尾的等差数列最大长度，需要额外记录遍历过的数字及索引。

```java
class Solution {
    public int longestArithSeqLength(int[] A) {
        if (A == null || A.length == 0) return 0;

        int max = 0;
        // 记录数字在数组中的索引
        int[] nums = new int[20001];
        Arrays.fill(nums, -1);
        // 表示以i、j结尾的等差数列最大长度
        int[][] dp = new int[A.length][A.length];
        
        for (int i = 0; i < A.length; i++) {
            for (int j = i + 1; j < A.length; j++) {
                // i、j等差数列的前一个数
                int preNum = 2 * A[i] - A[j];
                // 不存在
                if (preNum < 0 || nums[preNum] == -1) dp[i][j] = 2;
                // 存在
                else dp[i][j] = dp[nums[preNum]][i] + 1;

                max = Math.max(max, dp[i][j]);
            }
            // 更新记录最近索引
            nums[A[i]] = i;
        }
        return max;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：48ms，在所有java提交中击败了94.16%的用户。

内存消耗：49.1MB，在所有java提交中击败了80.12%的用户。

## 官方解题

&emsp;暂无，密切关注。