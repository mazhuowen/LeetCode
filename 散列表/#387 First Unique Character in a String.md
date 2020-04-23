[toc]

Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.



**Note:** You may assume the string contain only lowercase letters.



## 题目解读

&emsp;寻找字符串中第一个唯一字符，返回索引或-1。

```java
class Solution {
    public int firstUniqChar(String s) {

    }
}
```

## 程序设计

* 采用哈希表记录计数，然后再次遍历返回第一个计数为1的字符。

```java
class Solution {
    public int firstUniqChar(String s) {
        if (s == null || s.length() == 0) return -1;

        char[] str = s.toCharArray();
        // 计数
        int[] counter = new int[26];
        for (char c : str) {
            counter[c - 'a']++;
        }
        // 判断
        for (int i = 0; i < str.length; i++) {
            if (counter[str[i] - 'a'] == 1) return i;
        }
        return -1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：5ms，在所有java提交中击败了93.16%的用户。

内存消耗：40.4MB，在所有java提交中击败了6.12%的用户。

## 官方解题

&emsp;同上。