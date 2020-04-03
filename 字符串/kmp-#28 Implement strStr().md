[toc]

Implement `strStr()`.

Return the index of the first occurrence of needle in haystack, or **-1** if needle is not part of haystack.

Clarification:

What should we return when `needle` is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return 0 when `needle` is an empty string. This is consistent to C's `strstr()` and Java's `indexOf()`.



## 题目解读

&emsp;查找子串是否出现在字符串中，没有则返回`-1`，否则返回索引。

```java
class Solution {
    public int strStr(String haystack, String needle) {

    }
}
```

## 程序设计

* 最简单的想法是从头遍历，每次截取后面的子串长度的字符串进行比较。

```java
class Solution {
    public int strStr(String haystack, String needle) {
        // 子串为空串
        if (needle == null || needle.isEmpty()) return 0;
        // 字符串为空或长度小于子串
        if (haystack == null || haystack.isEmpty() || haystack.length() < needle.length()) return -1;

        // 遍历判断是否相等
        int m = haystack.length(), n = needle.length();
        for (int i = 0; i <= m - n; i++) {
            if (haystack.substring(i, i + n).equals(needle)) return i;
        }
        return -1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O((N - M)M)$，空间复杂度为$O(1)$。其中$M$是子串的长度，$N$为字符串的长度。

执行用时：1ms，在所有java提交中击败了78.55%的用户。

内存消耗：39.8MB，在所有java提交中击败了5.06%的用户。

## 官方解题

