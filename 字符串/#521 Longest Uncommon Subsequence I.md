[toc]

Given two strings `a` and `b`, find the length of the **longest uncommon subsequence** between them.

A **subsequence** of a string `s` is a string that can be obtained after deleting any number of characters from `s`. For example, `"abc"` is a subsequence of `"aebdc"` because you can delete the underlined characters in `"aebdc"` to get `"abc"`. Other subsequences of `"aebdc"` include `"aebdc"`, `"aeb"`, and `""` (empty string).

An **uncommon subsequence** between two strings is a string that is a **subsequence of one but not the other**.

Return the length of the **longest uncommon subsequence** between `a` and `b`. If the longest uncommon subsequence doesn't exist, return $-1$.



**Constraints**:

* $1 \le \text{a.length, b.length} \le 100$
* `a` and `b` consist of lower-case English letters.



## 题目解读

&emsp;最长特殊序列定义如下：该序列为某字符串独有的最长子序列。找出两个字符串的最长特殊子序列。

```java
class Solution {
    public int findLUSlength(String a, String b) {

    }
}
```

## 程序设计

* 如果两个字符串相等，则所有子序列都相等，无不同子序列，返回`-1`，如果字符串在同一索引处存在不相等的字符或长度不等，则选择不相等的字符和尽可能长的字符串，即不管任何情况选择最长的字符串即可。

```java
class Solution {
    public int findLUSlength(String a, String b) {
        return a.equals(b) ? -1 : Math.max(a.length(), b.length());
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\max(N,M))$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.2MB，在所有java提交中击败了98.00%的用户。

## 官方解题

&emsp;官方思路同上。