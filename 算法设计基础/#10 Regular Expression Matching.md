[toc]

Given an input string (`s`) and a pattern (`p`), implement regular expression matching with support for `'.'` and `'*'`.

* `'.'` Matches any single character.
* `'*'` Matches zero or more of the preceding element.

The matching should cover the **entire** input string (not partial).



Note:

* `s` could be empty and contains only lowercase letters `a-z`.
* `p` could be empty and contains only lowercase letters `a-z`, and characters like `.` or `*`.



## 题目解读

&emsp;给定字符串和正则表达式，判断是否匹配。

```java
class Solution {
    public boolean isMatch(String s, String p) {

    }
}
```

## 程序设计

* 同时遍历两个字符串，如果字母相等或正则表达式是`.`，其后没有`*`，则可继续遍历下一个字母。
* 复杂点在于`*`与字母及`.`的组合可以表示零个或多个字母。这样当前字母匹配到`x*`的组合，分三种情况，第一种视`x*`为零，当前字母与其后的字母继续匹配；第二种情况当前字母匹配`x*`，下一个字母继续匹配`x*`，表示`x*`匹配多个；第三种情况当前字母匹配，下一个字母匹配下一个，即`x*`只匹配一个字母。
* 还需要注意最后匹配结束，正则表达式还余有一部分`x*`模式的判断。

```java
class Solution {
    public boolean isMatch(String s, String p) {
        return isMatch(s, p, 0, 0);
    }

    private boolean isMatch(String s, String p, int l, int r) {
        if (s.length() == l) {
            while (r < p.length()) {
                if (r + 1 >= p.length() || p.charAt(r + 1) != '*') break;
                r += 2;
            }
            if (p.length() == r) return true;
            return false;
        }
        if (p.length() == r) return false;

        // 匹配所有字符
        if (p.charAt(r) == '.') {
            // s后的字符继续匹配.*，及s后的字符匹配.*后的字符两种情况
            if (r + 1 < p.length() && p.charAt(r + 1) == '*') {
                return isMatch(s, p, l, r + 2) || isMatch(s, p, l + 1, r) || isMatch(s, p, l + 1, r + 2);
            }
            // 继续向后匹配
            else {
                return isMatch(s, p, l + 1, r + 1);
            }
        }
        // 单词
        else {
            if (r + 1 < p.length() && p.charAt(r + 1) == '*') {
                if (p.charAt(r) == s.charAt(l)) return isMatch(s, p, l, r + 2) || isMatch(s, p, l + 1, r) || isMatch(s, p, l + 1, r + 2);
                else return isMatch(s, p, l, r + 2);
            } else {
                // 相等，匹配下一个
                if (p.charAt(r) == s.charAt(l)) return isMatch(s, p, l + 1, r + 1);
                    // 不相等，返回
                else return false;
            }
        }
    }
}
```

* 仔细观察`isMatch(s, p, l, r + 2) || isMatch(s, p, l + 1, r) || isMatch(s, p, l + 1, r + 2)`，实际上第三个条件是重复的前两个，可以去掉。参考官方思路，可以将上面代码简化为如下形式：

```java
class Solution {
    public boolean isMatch(String s, String p) {
        return isMatch(s, p, 0, 0);
    }

    private boolean isMatch(String s, String p, int l, int r) {
        // 模式已遍历完，返回字符串是否遍历完
        if (p.length() == r) return s.length() <= l;

        // 当前字符是否匹配
        boolean isCurMatch = l < s.length() && (s.charAt(l) == p.charAt(r) || p.charAt(r) == '.');

        // 当前模式是`x×`模式
        if (r + 1 < p.length() && p.charAt(r + 1) == '*') {
            // 1、如果当前字符匹配，下一个字符和当前模式继续匹配；
            // 2、不管当前字符是否与模式匹配，将`x×`看做0字符，继续与下一个模式匹配
            return (isCurMatch && isMatch(s, p, l + 1, r)) || isMatch(s, p, l, r + 2);
        }
        // 模式为单字符，如果当前匹配则继续匹配下一个字符
        else {
            return isCurMatch && isMatch(s, p, l + 1, r + 1);
        }
    }
}
```

## 性能分析

&emsp;回溯法最好的情况不存在`*`号，则算法就是简单的遍历比较，时间复杂度为$O(\min(M,N))$，最坏的情况假设遍历匹配字符串的长度为$i$，模式字符串前面全是`x*`模式，假设遍历的模式长度为$j$，则$j \ge i$，遍历到当前位置需要两个字符串移动$i + j$次，而在最坏情况`isMatch`只会同时移动一个字符串的位置，即调用` isMatch(s, p, l + 1, r)`方法$i$次，调用` isMatch(s, p, l, r + 2)`方法$j$次，调用次序有$C_{i + j}^i$种方案，即调用`isMatch`有$C_{i + j}^i$次，总的时间复杂度为$\sum_{i = 0}^M\sum_{j = 0}^{\frac{N}{2}}Ｃ_{i + j}^iO(M + N - i - 2j) \approx O((M + N)2^{M + \frac{N}{2}})$；空间复杂度为$O((M + N)2^{M + \frac{N}{2}})$。其中$M$为匹配字符串长度，$N$为模式字符串长度。

执行用时：37ms，在所有java提交中击败了43.30%的用户。

内存消耗：38.1MB，在所有java提交中击败了34.18%的用户。

## 官方解题

&emsp;除了上述思路，官方还提供了动态规划的方法。在上述分析中会发现`isMatch(s, p, l + 1, r))`，`isMatch(s, p, l, r + 2)`都可能会走`isMatch(s, p, l + 1, r + 2))`，存在重复计算的问题，可以使用动态规划优化。只需在上面基础上添加动态规划数组即可。

```java
class Solution {
    // ０表示未关联，１表示可以匹配，－１表示不可匹配
    int[][] dp;

    public boolean isMatch(String s, String p) {
        // 长度加一，防止空字符串
        dp = new int[s.length() + 1][p.length() + 1];
        return isMatch(s, p, 0, 0);
    }

    private boolean isMatch(String s, String p, int l, int r) {
        // 已有结果，返回
        if (dp[l][r] != 0) return dp[l][r] == 1;

        // 模式已遍历完，返回字符串是否遍历完
        if (p.length() == r) {
            dp[l][r] = 1;
            return s.length() <= l;
        }

        // 当前字符是否匹配
        boolean isCurMatch = l < s.length() && (s.charAt(l) == p.charAt(r) || p.charAt(r) == '.');

        // 当前模式是`x×`模式
        if (r + 1 < p.length() && p.charAt(r + 1) == '*') {
            // 1、如果当前字符匹配，下一个字符和当前模式继续匹配；
            // 2、不管当前字符是否与模式匹配，将`x×`看做0字符，继续与下一个模式匹配
            if ((isCurMatch && isMatch(s, p, l + 1, r)) || isMatch(s, p, l, r + 2)) {
                dp[l][r] = 1;
            } else {
                dp[l][r] = -1;
            }
        }
        // 模式为单字符，如果当前匹配则继续匹配下一个字符
        else {
            if (isCurMatch && isMatch(s, p, l + 1, r + 1)) {
                dp[l][r] = 1;
            } else {
                dp[l][r] = -1;
            }
        }
        return dp[l][r] == 1;
    }
}
```

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：2ms，在所有java提交中击败了99.98%的用户。

内存消耗：39.9MB，在所有java提交中击败了24.74%的用户。