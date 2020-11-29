[toc]

Given a string `S`, find the number of different non-empty palindromic subsequences in `S`, and **return that number modulo** $10^9 + 7$.

A subsequence of a string `S` is obtained by deleting $0$ or more characters from `S`.

A sequence is palindromic if it is equal to the sequence reversed.

Two sequences `A_1, A_2, ...` and `B_1, B_2, ...` are different if there is some i for which `A_i != B_i`.



**Note**:

* The length of `S` will be in the range `[1, 1000]`.
* Each character `S[i]` will be in the set `{'a', 'b', 'c', 'd'}`.



## 题目解读

&emsp;返回不重复的回文子序列数目。

```java
class Solution {
    public int countPalindromicSubsequences(String S) {

    }
}
```

## 程序设计

* 动态规划数组保存区间内以字母`a`、`b`、`c`、`d`结尾的不重复回文串数目。如果当前区间首尾字符`x`相等，则可构成`x`、`xx`及拼接中间所有不重复回文串的字符串`x…x`形成新的不重复回文串，即`dp(i,j,x) = 2 + dp(i+1, j-1, a) + dp(i+1, j-1, b) + dp(i+1, j-1, c) + dp(i+1, j-1, d)`；如果首尾不相等，分别为`x`、`y`，则`dp(i,j,x) = dp(i+1,j,x)`，`dp(i,j,y)=dp(i,j-1,y)`，剩余的两个字母和子区间一致，即`dp(i,j,r) = dp(i+1,j-1,r)`。

```java
class Solution {
    int mod = 1_000_000_007;

    public int countPalindromicSubsequences(String S) {
        if (S == null || S.isEmpty()) return 0;

        int n = S.length();
        char[] str = S.toCharArray();

        // 表示区间i～j之间以a、b、c、d结尾的回文字符串种数
        int[][][] dp = new int[n][n][4];

        for (int start = n - 1; start >= 0; start--) {
            // 只有一个字符的序列初始化为１
            dp[start][start][str[start] - 'a'] = 1;
            for (int end = start + 1; end < n; end++) {

                // 首尾能够组成新的回文子序列
                if (str[start] == str[end]) {
                    // 首尾组成两个长度的序列、首尾选一个组成一个长度的序列两种情况；即首尾之间忽略其它序列的情况
                    dp[start][end][str[start] - 'a'] = 2;
                    // 首尾加中间回文序列数目
                    for (int i = 0; i < 4; i++) {
                        dp[start][end][str[start] - 'a'] += dp[start + 1][end - 1][i];
                        dp[start][end][str[start] - 'a'] %= mod;
                    }
                    // 更新其他字符
                    for (int i = 0; i < 4; i++) {
                        if (str[start] - 'a' == i) continue;
                        dp[start][end][i] = dp[start + 1][end - 1][i];
                    }
                } else {
                    dp[start][end][str[start] - 'a'] = dp[start][end - 1][str[start] - 'a'];
                    dp[start][end][str[end] - 'a'] = dp[start + 1][end][str[end] - 'a'];
                    // 更新其他字符
                    for (int i = 0; i < 4; i++) {
                        if (str[start] - 'a' == i || str[end] - 'a' == i) continue;
                        dp[start][end][i] = dp[start + 1][end - 1][i];
                    }
                }
            }
        }
        int count = 0;
        for (int i = 0; i < 4; i++) {
            count += dp[0][n - 1][i];
            count %= mod;
        }
        return count;
    }
}
```

* 注意到每次动态规划更新都只与当前行和下一行有关，可以将动态规划数组降低为二维；其次由于字母是固定4个，可以继续降低为一维数组。

```java
class Solution {
    int mod = 1_000_000_007;

    public int countPalindromicSubsequences(String S) {
        if (S == null || S.isEmpty()) return 0;

        int n = S.length();
        char[] str = S.toCharArray();

        // 当前行i，列为j，表示区间i～j之间以a、b、c、d结尾的回文字符串种数
        int[] cur = new int[4 * n];
        int[] next = new int[4 * n];

        for (int start = n - 1; start >= 0; start--) {
            char s = str[start];
            int sIdx = s - 'a';
            // 只有一个字符的序列初始化为１
            cur[start * 4 + sIdx] = 1;
            for (int end = start + 1; end < n; end++) {
                char e = str[end];
                int eIdx = e - 'a';
                // 首尾能够组成新的回文子序列
                if (s == e) {
                    // 首尾组成两个长度的序列、首尾选一个组成一个长度的序列两种情况；即首尾之间忽略其它序列的情况
                    cur[end * 4 + sIdx] = 2;
                    // 首尾加中间回文序列数目
                    for (int i = 0; i < 4; i++) {
                        cur[end * 4 + sIdx] += next[(end - 1) * 4 + i];
                        cur[end * 4 + sIdx] %= mod;
                    }
                    // 更新其他字符
                    for (int i = 0; i < 4; i++) {
                        if (i == sIdx) continue;
                        cur[end * 4 + i] = next[(end - 1) * 4 + i];
                    }
                } else {
                    cur[end * 4 + sIdx] = cur[(end - 1) * 4 + sIdx];
                    cur[end * 4 + eIdx] = next[end * 4 + eIdx];
                    // 更新其他字符
                    for (int i = 0; i < 4; i++) {
                        if (sIdx == i || eIdx == i) continue;
                        cur[end * 4 + i] = next[(end - 1) * 4 + i];
                    }
                }
            }
            next = cur;
            cur = new int[4 * n];
        }
        int count = 0;
        for (int i = 0; i < 4; i++) {
            count += next[(n - 1) * 4 + i];
            count %= mod;
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：256ms，在所有java提交中击败了13.51%的用户。

内存消耗：126.7MB，在所有java提交中击败了100.00%的用户。

&emsp;优化后时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：58ms，在所有java提交中击败了63.51%的用户。

内存消耗：40.1MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。