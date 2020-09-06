[toc]

Given a string `s` containing only lower case English letters and the '?' character, convert **all** the '?' characters into lower case letters such that the final string does not contain any **consecutive repeating** characters. You **cannot** modify the non '?' characters.

It is **guaranteed** that there are no consecutive repeating characters in the given string **except** for '?'.

Return the final string after all the conversions (possibly zero) have been made. If there is more than one solution, return any of them. It can be shown that an answer is always possible with the given constraints.



**Constraints**:

* $1 \le \text{s.length} \le 100$
* `s` contains only lower case English letters and `'?'`.



## 题目解读

&emsp;替换字符串中的`?`为任意字母，使得相邻的字母不重复。

```java
class Solution {
    public String modifyString(String s) {

    }
}
```

## 程序设计

* 当前问号替换取决于前后字符的影响，在得到前后字符后，只需从`a`开始迭代，得到一个和前后字符不相等的字母即可。

```java
class Solution {
    public String modifyString(String s) {
        if (s == null || s.isEmpty()) return "";
        char[] seq = s.toCharArray();
        for (int i = 0; i < seq.length; i++) {
            if (seq[i] != '?') continue;
            
            // 当前问号的前一字符（如果是首位，不存在，则赋值标识符）
            char pre = i == 0 ? '^' : seq[i - 1];
            // 当前问号的后一字符（如果后一位不存在或是问号，则赋予标识符）
            char next = (i == seq.length - 1 || seq[i + 1] == '?') ? '$' : seq[i + 1];

            char c = 'a';
            while (c == pre || c == next) c++;
            seq[i] = c;
        }
        return new String(seq);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。



## 官方解题

&emsp;