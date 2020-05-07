[toc]

Given two strings `text1` and `text2`, return the length of their longest common subsequence.

A subsequence of a string is a new string generated from the original string with some characters(can be none) deleted without changing the relative order of the remaining characters. (eg, "ace" is a subsequence of "abcde" while "aec" is not). A common subsequence of two strings is a subsequence that is common to both strings.

 

If there is no common subsequence, return 0.



Constraints:

* $1 \le \text{text1.length} \le 1000$
* $1 \le \text{text2.length} \le 1000$
* The input strings consist of lowercase English characters only.



## 题目解读

&emsp;最长公共子序列的长度。

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {

    }
}
```

## 程序设计

* 动态规划数组`dp(i,j)`表示截止字符串1`i`的位置和字符串2`j`的位置最长公共子串长度。

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        if (text1 == null || text2 == null || text1.isEmpty() || text2.isEmpty()) return 0;

        int m = text1.length(), n = text2.length();
        int[][] dp = new int[m + 1][n + 1];

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (text1.charAt(i) == text2.charAt(j)) dp[i + 1][j + 1] = dp[i][j] + 1;
                else dp[i + 1][j + 1] = Math.max(dp[i][j + 1], dp[i + 1][j]);
            }
        }
        return dp[m][n];
    }
}
```

* 优化空间：

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        if (text1 == null || text2 == null || text1.isEmpty() || text2.isEmpty()) return 0;

        int m = text1.length(), n = text2.length();
        int[] cur = new int[n + 1];
        int[] pre = new int[n + 1];

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (text1.charAt(i) == text2.charAt(j)) cur[j + 1] = pre[j] + 1;
                else cur[j + 1] = Math.max(pre[j + 1], cur[j]);
            }
            pre = cur;
            cur = new int[n + 1];
        }
        return pre[n];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：13ms，在所有java提交中击败了69.28%的用户。

内存消耗：43.7MB，在所有java提交中击败了8.33%的用户。

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(N)$。

执行用时：12ms，在所有java提交中击败了74.82%的用户。

内存消耗：39.9MB，在所有java提交中击败了8.33%的用户。

## 官方解题

&emsp;暂无，密切关注。社区思路类似。