[toc]

Given a string `s1`, we may represent it as a binary tree by partitioning it to two non-empty substrings recursively.

Below is one possible representation of `s1 = "great"`:

        great
       /    \
      gr    eat
     / \    /  \
    g   r  e   at
               / \
              a   t

To scramble the string, we may choose any non-leaf node and swap its two children.

For example, if we choose the node `"gr"` and swap its two children, it produces a scrambled string `"rgeat"`.

        rgeat
       /    \
      rg    eat
     / \    /  \
    r   g  e   at
               / \
              a   t

We say that `"rgeat"` is a scrambled string of `"great"`.

Similarly, if we continue to swap the children of nodes `"eat"` and `"at"`, it produces a scrambled string `"rgtae"`.

        rgtae
       /    \
      rg    tae
     / \    /  \
    r   g  ta  e
           / \
          t   a

We say that `"rgtae"` is a scrambled string of `"great"`.

Given two strings `s1` and `s2` of the same length, determine if `s2` is a scrambled string of `s1`.



## 题目解读

&emsp;判断是否是扰乱字符。

```java
class Solution {
    public boolean isScramble(String s1, String s2) {

    }
}
```

## 程序设计

* 借鉴社区解法，动态规划`dp(i,j,k)`表示字符串`s`从`i`开始，字符串`t`从`j`开始长度为`k`的字符串是否为扰乱字符号串。
* 对于长度为`len`的子字符串，存在两种情况：
  * `s`分为长度为`k`的`s1`和长度为`len-k`的`s2`，`t`同样分为同样长度的`t1`和`t2`，其中`s1`与`t1`、`s2`与`t2`互为扰乱字符串，则`s`与`t`为扰乱字符串。
  * `s`分为长度为`k`的`s1`和长度为`len-k`的`s2`，`t`分为长度为`len-k`的`t1`和长度为`k`的`t2`，其中`s1`与`t2`、`s2`与`t1`互为扰乱字符串，则`s`与`t`为扰乱字符串。

```java
class Solution {
    public boolean isScramble(String s1, String s2) {
        if (s1 == null || s2 == null) throw new IllegalArgumentException("invalid param");

        char[] c1 = s1.toCharArray(), c2 = s2.toCharArray();
        if (c1.length != c2.length) return false;

        int n = c1.length;
        boolean[][][] dp = new boolean[n][n][n + 1];

        // 初始化
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                dp[i][j][1] = c1[i] == c2[j];
            }
        }

        for (int len = 2; len <= n; len++) {
            for (int i = 0; i <= n - len; i++) {
                for (int j = 0; j <= n - len; j++) {
                    for (int k = 1; k <= len - 1; k++) {
                        // s1与t1、s2与t2为扰乱字符串，则s与t为扰乱字符串
                        if (dp[i][j][k] && dp[i + k][j + k][len - k]) {
                            dp[i][j][len] = true;
                            break;
                        }
                        // s1与t2、s2与t1为扰乱字符串，则s与t为扰乱字符串
                        if (dp[i][j + len - k][k] && dp[i + k][j][len - k]) {
                            dp[i][j][len] = true;
                            break;
                        }
                    }
                }
            }
        }
        return dp[0][0][n];
    }
}
```

* 有了上述递归思路，可得到记忆化的回溯思路：

```java
class Solution {
    int[][][] record;

    public boolean isScramble(String s1, String s2) {
        if (s1 == null || s2 == null) throw new IllegalArgumentException("invalid param");
        // 长度不等
        if (s1.length() != s2.length()) return false;

        int n = s1.length();
        // 1表示是扰乱字符串，-1表示不是
        this.record = new int[n][n][n + 1];
        return isScramble(s1.toCharArray(), 0, n - 1, s2.toCharArray(), 0, n - 1);
    }

    private boolean isScramble(char[] s1, int start1, int end1, char[] s2, int start2, int end2) {
        // 存在记录
        if (record[start1][start2][end1 - start1 + 1] != 0)
            return record[start1][start2][end1 - start1 + 1] == 1;

        // 相等则返回
        if (isSame(s1, start1, end1, s2, start2, end2)) {
            record[start1][start2][end1 - start1 + 1] = 1;
            return true;
        }

        // 判断字符是否新等
        int[] counter = new int[26];
        for (int i = start1; i <= end1; i++) counter[s1[i] - 'a']++;
        for (int i = start2; i <= end2; i++) {
            counter[s2[i] - 'a']--;
            // 字符不等
            if (counter[s2[i] - 'a'] < 0) {
                record[start1][start2][end1 - start1 + 1] = -1;
                return false;
            }
        }

        boolean flag = false;
        for (int i = start1; i < end1; i++) {
            // 情况1
            if (isScramble(s1, start1, i, s2, start2, start2 + i - start1)
                && isScramble(s1, i + 1, end1, s2, start2 + i - start1 + 1, end2)) {
                    flag = true;
                    break;
                }

            // 情况2
            if (isScramble(s1, start1, i, s2, end2 + start1 - i, end2)
                && isScramble(s1, i + 1, end1, s2, start2, end2 + start1 - i - 1)) {
                    flag = true;
                    break;
                }
        }
        record[start1][start2][end1 - start1 + 1] = (flag ? 1 : -1);
        return flag;
    }

    private boolean isSame(char[] s1, int start1, int end1, char[] s2, int start2, int end2) {
        while (start1 <= end1) {
            if (s1[start1++] != s2[start2++]) return false;
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^4)$，空间复杂度为$O(N^3)$。

执行用时：8ms，在所有java提交中击败了53.97%的用户。

内存消耗：39.6MB，在所有java提交中击败了44.44%的用户。

&emsp;记忆化回溯时间复杂度为$O(N^2)$，空间复杂度为$O(N^3)$。

执行用时：3ms，在所有java提交中击败了83.40%的用户。

内存消耗：40.2MB，在所有java提交中击败了22.22%的用户。

## 官方解题

&emsp;暂无，密切关注。