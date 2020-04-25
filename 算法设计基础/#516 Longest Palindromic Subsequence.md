[toc]

Given a string s, find the longest palindromic subsequence's length in s. You may assume that the maximum length of s is 1000.



## 题目解读

&emsp;返回字符串最大回文子序列，非子串，子序列是原序列中保持前后相对顺序的序列，不一定紧邻。

```java
class Solution {
    public int longestPalindromeSubseq(String s) {

    }
}
```

## 程序设计

* 采用动态规划，记录区间内最大的回文子串长度。

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        if (s == null || s.isEmpty()) return 0;

        int n = s.length();
        int[][] dp = new int[n][n];
        for (int len = 1; len <= n; len++) {
            for (int start = 0, end = start + len - 1; end < n; start++, end++) {
                if (len == 1) dp[start][end] = 1;
                else if (len == 2) dp[start][end] = s.charAt(start) == s.charAt(end) ? 2 : 1;
                else {
                    dp[start][end] = (s.charAt(start) == s.charAt(end) ? 2 : 0) + dp[start + 1][end - 1];
                    dp[start][end] = Math.max(dp[start][end], dp[start][end - 1]);
                    dp[start][end] = Math.max(dp[start][end], dp[start + 1][end]);
                }
            }
        }
        return dp[0][n - 1];
    }
}
```

* 简化上述逻辑

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        if (s == null || s.isEmpty()) return 0;

        int n = s.length();
        int[][] dp = new int[n][n];
        for (int len = 1; len <= n; len++) {
            for (int start = 0, end = start + len - 1; end < n; start++, end++) {
                if (len == 1) dp[start][end] = 1;
                else if (len == 2) dp[start][end] = s.charAt(start) == s.charAt(end) ? 2 : 1;
                else {
                    // 简化此处逻辑
                    if (s.charAt(start) == s.charAt(end)) {
                        // 此时形成的回文串不小于dp[start][end - 1], dp[start + 1][end]，无需比较
                        dp[start][end] = dp[start + 1][end - 1] + 2;
                    } else {
                        dp[start][end] = Math.max(dp[start][end - 1], dp[start + 1][end]);
                    }
                }
            }
        }
        return dp[0][n - 1];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：70ms，在所有java提交中击败了13.93%的用户。

内存消耗：50.1MB，在所有java提交中击败了6.90%的用户。

&emsp;优化后性能如下：

执行用时：51ms，在所有java提交中击败了29.80%的用户。

内存消耗：50.2MB，在所有java提交中击败了6.90%的用户。

## 官方解题

&emsp;暂无，密切关注。参考社区优化思路，逐行从后往前更新，同时使用字符数组加速遍历。

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        if (s == null || s.isEmpty()) return 0;

        int n = s.length();
        char[] str = s.toCharArray();
        int[][] dp = new int[n][n];
        for (int start = n - 1; start >= 0; start--) {
            dp[start][start] = 1;
            for (int end = start + 1; end < n; end++) {
                if (str[start] == str[end]) {
                    dp[start][end] = dp[start + 1][end - 1] + 2;
                } else {
                    dp[start][end] = Math.max(dp[start][end - 1], dp[start + 1][end]);
                }
            }
        }

        return dp[0][n - 1];
    }
}
```

执行用时：25ms，在所有java提交中击败了84.84%的用户。

内存消耗：50.1MB，在所有java提交中击败了6.90%的用户。

&emsp;观察上述代码，发现每次动态规划都与本行及下一行有关，可以压缩动态规划数组为一维。

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        if (s == null || s.isEmpty()) return 0;

        int n = s.length();
        char[] str = s.toCharArray();
        int[] pre = new int[n];
        int[] next = new int[n];
        // 从后往前更新
        for (int start = n - 1; start >= 0; start--) {
            pre[start] = 1;
            for (int end = start + 1; end < n; end++) {
                if (str[start] == str[end]) {
                    pre[end] = next[end - 1] + 2;
                } else {
                    pre[end] = Math.max(pre[end - 1], next[end]);
                }
            }
            next = pre;
            pre = new int[n];
        }

        return next[n - 1];
    }
}
```

&emsp;空间复杂度降为$O(N)$。

执行用时：19ms，在所有java提交中击败了96.36%的用户。

内存消耗：39.9MB，在所有java提交中击败了96.55%的用户。