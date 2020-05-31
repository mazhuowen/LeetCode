[toc]

Given a string `s`. In one step you can insert any character at any index of the string.

Return the minimum number of steps to make `s` palindrome.

A **Palindrome String** is one that reads the same backward as well as forward.



Constraints:

* $1 \le \text{s.length} \le 500$
* All characters of `s` are lower case English letters.



## 题目解读

&emsp;给定字符串，可插入任意位置任意字符，求使得字符串为回文串的最小操作数。

```java
class Solution {
    public int minInsertions(String s) {

    }
}
```

## 程序设计

* 动态规划数组`dp(i,j)`表示子字符串`i~j`变换为回文串所需最小操作数，则分为两种情况：
  * `i`、`j`字符相等，则`dp(i,j) = dp(i - 1, j - 1)`；
  * `i`、`j`不相等，则需要在`i~j-1`的基础上在`i`前插入`j`所对应的字符或在`i+1~j`的基础上在`j`后插入`i`所对应的字符；即选取两种方案中的较小值。

```java
class Solution {
    public int minInsertions(String s) {
        if (s == null || s.isEmpty()) return 0;

        int n = s.length();
        char[] str = s.toCharArray();
        // 表示i~j子字符串的最小操作数
        int[][] dp = new int[n][n];
        
        for (int len = 2; len <= n; len++) {
            for (int start = 0, end = start + len - 1; end < n; start++, end++) {
                // 首尾字符相等
                if (str[start] == str[end]) dp[start][end] = dp[start + 1][end - 1];
                // 不相等，则选取较小值
                else dp[start][end] = Math.min(dp[start + 1][end], dp[start][end - 1]) + 1;
            }
        }

        return dp[0][n - 1];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：17ms，在所有java提交中击败了84.49%的用户。

内存消耗：41MB，在所有java提交中击败了6.00%的用户。

## 官方解题

&emsp;同上。