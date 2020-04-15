[toc]

Given a string s consists of upper/lower-case alphabets and empty space characters `' '`, return the length of last word (last word means the last appearing word if we loop from left to right) in the string.

If the last word does not exist, return 0.

Note: A word is defined as a **maximal substring** consisting of non-space characters only.



## 题目解读

&emsp;返回最后一个单词长度。

```java
class Solution {
    public int lengthOfLastWord(String s) {

    }
}
```

## 程序设计

* 从后遍历到第一个空格，记录长度并返回。需要考虑特殊情况，即最后是一个或一段空格的情况。

```java
class Solution {
    public int lengthOfLastWord(String s) {
        if (s == null || s.isEmpty()) return 0;
        int n = s.length();
        int len = 0;
        for (int i = n - 1; i >= 0; i --) {
            if (s.charAt(i) == ' ' ) {
                // 如果len大于0，遇到空格表示单词结束，返回
                // 否则空格在字符串末尾，继续向前遍历
                if (len > 0) break;
                else continue;
            }
            len++;
        }
        return len;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.4MB，在所有java提交中击败了6.38%的用户。

## 官方解题

&emsp;暂无，密切关注。