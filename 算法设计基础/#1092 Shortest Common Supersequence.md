[toc]

Given two strings `str1` and `str2`, return the shortest string that has both `str1` and `str2` as subsequences.  If multiple answers exist, you may return any of them.

(A string S is a subsequence of string T if deleting some number of characters from T (possibly 0, and the characters are chosen anywhere from T) results in the string S.)



Note:

* $1 \le \text{str1.length, str2.length} \le 1000$
* `str1` and `str2` consist of lowercase English letters.



## 题目解读

&emsp;求最短的子夫差un，使得给出的两个字符串都是给字符串的子序列。

```java
class Solution {
    public String shortestCommonSupersequence(String str1, String str2) {

    }
}
```

## 程序设计

* 参考社区思路，首先求解最长公共子序列，然后将两个字符串插入公共子序列得到结果。

```java
class Solution {
    public String shortestCommonSupersequence(String str1, String str2) {
        if (str1 == null || str1.isEmpty()) return str2;
        if (str2 == null || str2.isEmpty()) return str1;

        char[] s1 = str1.toCharArray(), s2 = str2.toCharArray();
        String lcs = longestCommonSubstring(s1, s2);

        // 根据最长公共子序列拼接
        int idx1 = 0, idx2 = 0;
        StringBuffer sb = new StringBuffer();
        for (char c : lcs.toCharArray()) {
            // 先拼接字符串1
            while (idx1 < s1.length && s1[idx1] != c) sb.append(s1[idx1++]);
            // 后拼接字符串2
            while (idx2 < s2.length && s2[idx2] != c) sb.append(s2[idx2++]);

            // 拼接当前字符
            sb.append(c);
            idx1++;
            idx2++;
        }
		// 拼接剩余字符
        while (idx1 < s1.length) sb.append(s1[idx1++]);
        while (idx2 < s2.length) sb.append(s2[idx2++]);
        
        return sb.toString();
    }

    // 最长公共子序列
    private String longestCommonSubstring(char[] str1, char[] str2) {
        String[][] dp = new String[str1.length + 1][str2.length + 1];
        dp[0][0] = "";
        
        for (int i = 0; i < str1.length; i++) {
            for (int j = 0; j < str2.length; j++) {
                if (i == 0) dp[i][j + 1] = "";
                if (j == 0) dp[i + 1][j] = "";

                if (str1[i] == str2[j]) {
                    dp[i + 1][j + 1] = dp[i][j] + str1[i];
                } else {
                    dp[i + 1][j + 1] = dp[i + 1][j].length() > dp[i][j + 1].length() ? dp[i + 1][j] : dp[i][j + 1];
                }
            }
        }
        return dp[str1.length][str2.length];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：29ms，在所有java提交中击败了62.14%的用户。

内存消耗：49MB，在所有java提交中击败了100.00%的用户。

内存消耗 :49 MB, 在所有 Java 提交中击败了100.00%的用户

## 官方解题

&emsp;暂无，密切关注。