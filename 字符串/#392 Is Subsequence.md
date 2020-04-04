[toc]

Given a string **s** and a string **t**, check if **s** is subsequence of **t**.

You may assume that there is only lower case English letters in both **s** and **t**. **t** is potentially a very long (length ~= 500,000) string, and **s** is a short string (<=100).

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, `"ace"` is a subsequence of `"abcde"` while `"aec"` is not).

Follow up:
If there are lots of incoming S, say S1, S2, ... , Sk where k >= 1B, and you want to check one by one to see if T has its subsequence. In this scenario, how would you change your code?



## 题目解读

&emsp;给定两个字符串，判断是否是子串。

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        
    }
}
```

## 程序设计

* 题目中子串的定义为维持原字符串中字符相对位置的子串。对于合法的子串，子串中的字符顺序在原字符串中也存在，可以按顺序遍历子串，然后在原串中查找是否存在，不存在则不合法。

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        // 空串是子串
        if (s == null || s.isEmpty()) return true; 
        if (t == null || t.isEmpty()) return false;

        char[] seq = t.toCharArray();
        char[] sub = s.toCharArray();

        int idx = 0;
        // 在原始字符串中查找子串字符
        for (char c : sub) {
            // 在原串中查找
            while (idx < seq.length && seq[idx] != c) idx++;
            // 不存在当前字符c，不合法
            if (idx == seq.length) return false;
            // 匹配成功，定位到下一个位置，继续下一轮比较
            idx++;
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：4ms，在所有java提交中击败了68.23%的用户。

内存消耗：44.8MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区时间性能较好的思路同上，实现上利用了`indexOf`函数，而非遍历查找。

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        // 空串是子串
        if (s == null || s.isEmpty()) return true; 
        if (t == null || t.isEmpty()) return false;

        int idx = -1;
        for (char c : s.toCharArray()) {
            idx = t.indexOf(c, idx + 1);
            if (idx == -1) return false;
        }
        return true;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了92.00%的用户。

内存消耗：42.8MB，在所有java提交中击败了100.00%的用户。