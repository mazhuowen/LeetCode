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

## 性能分析

执行用时：1852ms，在所有java提交中击败了5.01%的用户。

内存消耗：38.2MB，在所有java提交中击败了33.73%的用户。

## 官方解题

