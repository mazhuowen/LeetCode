[toc]

Given an input string, reverse the string word by word.



Note:

* A word is defined as a sequence of non-space characters.
* Input string may contain leading or trailing spaces. However, your reversed string should not contain leading or trailing spaces.
* You need to reduce multiple spaces between two words to a single space in the reversed string.



## 题目解读

&emsp;翻转字符，在翻转的过程中删除单词之间多余的空格。

```java
class Solution {
    public String reverseWords(String s) {
        
    }
}
```

## 程序设计

* 主要在于翻转后将单词之间多余的空格删除。

```java
class Solution {
    public String reverseWords(String s) {
        if (s == null || s.isEmpty()) return s;

        String[] strs = s.split(" ");
        StringBuffer sb = new StringBuffer();
        for (int i = strs.length - 1; i >= 0; i--) {
            // 删除多余的空格
            if (strs[i].isEmpty()) continue;
            sb.append(strs[i]).append(" ");
        }
        // 删除最后拼接的空格
        return sb.toString().trim();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了99.99%的用户。

内存消耗：39.8MB，在所有java提交中击败了5.26%的用户。

## 官方解题

&emsp;官方解题手动实现方法。