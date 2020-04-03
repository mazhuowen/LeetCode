[toc]

A message containing letters from `A-Z` is being encoded to numbers using the following mapping:

```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

Given a **non-empty** string containing only digits, determine the total number of ways to decode it.



## 题目解读

&emsp;每个字母对应一个数字编码，给定一个数字序列，返回所有可能的字母序列的数目

```java
class Solution {
    public int numDecodings(String s) {

    }
}
```

## 程序设计

* 每个字母编码对应从1到26的数字，由于编码最多有两位，则序列中的每个数字首先可以对应一个编码，如果和前边的数字可以组成26以内的数字，则前面的数字又可以组合为一个编码。假设考虑相连的三个数字的序列`abc`，截止`a`存在的编码数目为$n$，截止`b`存在的编码数是$m$，对于`c`如果直接转为数字对于的字母，是一种编码方式，此时有$m$中编码方式；如果`c`能和`b`组成26以内的数字，可以将`bc`视为一个字母，此时对应编码方式为从`a`开始的数目，即$n$种。
* 通过上述分析可采用动态规划记录前两个字母的编码种类。
* 除了总体思路，需要特别注意边角情况。注意到编码没有数字0，但存在数字10，单独的0是无法编码的，这意味需要判断0是否和之前的数字组成10或20，否则无法构成编码，返回0。

```java
class Solution {
    public int numDecodings(String s) {
        if (s == null || s.isEmpty()) return 0;

        int len = s.length();
        int[] dp = new int[len];
        char[] c = s.toCharArray();

        for (int i = 0; i < len; i++) {
            // 存在非法字母
            if (c[i] < '0' || c[i] > '9') return 0;

            // 0与前面字母组不成编码，返回
            if (c[i] == '0' && (i == 0 || c[i - 1] == '0' || c[i - 1] > '2')) return 0;

            // 初始化
            if (i == 0) {
                dp[0] = 1;
                continue;
            }

            // 当前数字和前面数字可构成26以内的数
            if (c[i - 1] == '1' || (c[i - 1] == '2' && c[i] <= '6')) {
                // 当前数字为0，则只能与前面数字编码
                if (c[i] == '0') {
                    dp[i] = i - 2 > 0 ? dp[i - 2] : 1;
                } 
                // 不为0，可与前面数字一起编码，也可单独编码
                else {
                    dp[i] = dp[i - 1] + (i - 2 > 0 ? dp[i - 2] : 1);
                }
            } 
            // 与前面数字构不成单独编码，只能单独编码
            else {
                dp[i] = dp[i - 1];
            }

        }

        return dp[c.length - 1];
    }
}
```

* 由上述分析可知只需记录前两个的编码数目，可在空间上优化：

```java
class Solution {
    public int numDecodings(String s) {
        if (s == null || s.isEmpty()) return 0;

        // 记录前面的两个数字对于编码方式数目
        int prePre = -1, pre = -1;
        char[] c = s.toCharArray();

        for (int i = 0; i < s.length(); i++) {
            // 存在非法字母
            if (c[i] < '0' || c[i] > '9') return 0;

            // 0与前面字母组不成编码，返回
            if (c[i] == '0' && (i == 0 || c[i - 1] == '0' || c[i - 1] > '2')) return 0;

            // 初始化
            if (i == 0) {
                pre = 1;
                continue;
            }
            int temp = pre;
            if (c[i - 1] == '1' || (c[i - 1] == '2' && c[i] <= '6')) {
                if (c[i] == '0') {
                    pre = prePre == -1 ? 1 : prePre;
                } else {
                    pre += prePre == -1 ? 1 : prePre;
                }
            }
            prePre = temp;
        }

        return pre;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$，优化后空间复杂度为$O(1)$

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.1MB，在所有java提交中击败了5.33%的用户。

## 官方解题

&emsp;暂无，密切关注。