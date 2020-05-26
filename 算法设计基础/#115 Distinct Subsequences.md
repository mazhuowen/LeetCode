[toc]

Given a string `S` and a string `T`, count the number of distinct subsequences of `S` which equals `T`.

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, `"ACE"` is a subsequence of `"ABCDE"` while `"AEC"` is not).

It's guaranteed the answer fits on a 32-bit signed integer.



## 题目解读

&emsp;给定一个字符串`S`和一个字符串`T`，计算在`S`的子序列中`T`出现的个数。

```java
class Solution {
    public int numDistinct(String s, String t) {
        
    }
}
```
## 程序设计
* 最简单的想到的是回溯，最坏情况下时间复杂度为$O(2^N)$，会超时。
```java
class Solution {
    int count = 0;

    public int numDistinct(String s, String t) {
        if (s == null || t == null) throw new IllegalArgumentException("invalid param");
        if (s.length() < t.length()) return 0;
        numDistinct(s.toCharArray(), -1, t.toCharArray(), -1);
        return count;
    }

    private void numDistinct(char[] s, int idx1, char[] t, int idx2) {
        if (idx2 + 1 == t.length) {
            count++;
            return;
        }

        for (int i = idx1 + 1; i < s.length; i++) {
            // 尝试
            if (s[i] == t[idx2 + 1]) {
                numDistinct(s, i, t, idx2 + 1);
            }
        }
    }
}
```
* 在上述基础上引入额外空间保存中间结果加速。

```java
class Solution {
    // 记录s的idx1之后及t的idx2之后匹配到的数目
    int[][] record;

    public int numDistinct(String s, String t) {
        if (s == null || t == null) throw new IllegalArgumentException("invalid param");
        if (s.length() < t.length()) return 0;

        this.record = new int[s.length() + 1][t.length() + 1];
        for (int[] temp : record) {
            Arrays.fill(temp, -1);
        }
        return numDistinct(s.toCharArray(), -1, t.toCharArray(), -1);
    }

    private int numDistinct(char[] s, int idx1, char[] t, int idx2) {
        if (idx2 + 1 == t.length) return 1;

        if (record[idx1 + 1][idx2 + 1] != -1) return record[idx1 + 1][idx2 + 1];

        record[idx1 + 1][idx2 + 1] = 0;
        for (int i = idx1 + 1; i < s.length; i++) {
            // 尝试
            if (s[i] == t[idx2 + 1]) {
                record[idx1 + 1][idx2 + 1] += numDistinct(s, i, t, idx2 + 1);
            }
        }
        return record[idx1 + 1][idx2 + 1];
    }
}
```

* 采用动态规划，`dp(i,j)`表示字符串`T`的前`i`个和`S`的前`j`个匹配数目；当前字符不相等时，`dp(i,j) = dp(i,j-1)`；当相等时，`dp(i,j)`为当前字符匹配的数目`dp(i-1,j-1)`和当前字符之前的匹配数目`dp(i,j-1)`之和。

```java
class Solution {

    public int numDistinct(String s, String t) {
        if (s == null || t == null) throw new IllegalArgumentException("invalid param");
        if (s.length() < t.length()) return 0;

        char[] str1 = s.toCharArray(), str2 = t.toCharArray();
        int[][] dp = new int[str2.length + 1][str1.length + 1];

        // T为空，则值为1
        Arrays.fill(dp[0], 1);

        for (int i = 0; i < str2.length; i++) {
            for (int j = 0; j < str1.length; j++) {
                if (str2[i] == str1[j]) {
                    dp[i + 1][j + 1] = dp[i + 1][j] + dp[i][j];
                } else {
                    dp[i + 1][j + 1] = dp[i + 1][j];
                }
            }
        }
        return dp[str2.length][str1.length];
    }
}
```

## 性能分析

&emsp;记忆化回溯时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：18ms，在所有java提交中击败了9.30%的用户。

内存消耗：39.7MB，在所有java提交中击败了12.50%的用户。

&emsp;动态规划时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：4ms，在所有java提交中击败了98.11%的用户。

内存消耗：40MB，在所有java提交中击败了12.50%的用户。

## 官方解题

&emsp;暂无，密切关注。