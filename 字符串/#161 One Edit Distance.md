[toc]

Given two strings **s** and **t**, determine if they are both one edit distance apart.

Note: 

There are 3 possiblities to satisify one edit distance apart:

* Insert a character into s to get t
* Delete a character from s to get t
* Replace a character of s to get t



## 题目解读

&emsp;判断两个字符串编辑距离是否是1。

```java
class Solution {
    public boolean isOneEditDistance(String s, String t) {

    }
}
```

## 程序设计

* 字符串遍历找到第一个不相等的字符，然后重新定位遍历，后续不能出现不相等的字符。

```java
class Solution {
    public boolean isOneEditDistance(String s, String t) {
        if (s == null || t == null) throw new IllegalArgumentException("invalid param");
        
        int m = s.length(), n = t.length();
        if (Math.abs(m - n) >= 2) return false;
        
        
        // 遍历查找第一个不相等的字符
        int idx = 0;
        while (idx < m && idx < n && s.charAt(idx) == t.charAt(idx)) {
            idx++;
        }
        // 根据三种情况重新定位遍历
        int idxS, idxT;
        if (m == n) {
            // 两个字符串相等，不符合要求
            if (idx == m) return false;
            idxS = idxT = idx + 1;
        } else if (m > n) {
            idxS = idx + 1;
            idxT = idx;
        } else {
            idxS = idx;
            idxT = idx + 1;
        }

        // 剩余部分不能存在不相等字符
        while (idxS < m) {
            if (s.charAt(idxS++) != t.charAt(idxT++)) return false;
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\max(M,N))$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了99.74%的用户。

内存消耗：38.5MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;同上，实现更简洁。