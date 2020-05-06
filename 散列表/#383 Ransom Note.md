[toc]

Given an arbitrary ransom note string and another string containing letters from all the magazines, write a function that will return true if the ransom note can be constructed from the magazines ; otherwise, it will return false.

Each letter in the magazine string can only be used once in your ransom note.

Note:
You may assume that both strings contain only lowercase letters.



## 题目解读

&emsp;给定字符串，判断是否可由另一个字符串中的字符构成。每个字符只能使用一次。

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        
    }
}
```

## 程序设计

* 使用散列表计数判断。

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        if (magazine == null || ransomNote == null) throw new IllegalArgumentException("invalid param");

        int[] counter = new int[26];
        for (char c : magazine.toCharArray()) {
            counter[c - 'a']++;
        }
        for (char c : ransomNote.toCharArray()) {
            counter[c - 'a']--;
            if (counter[c - 'a'] < 0) return false;
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(M + N)$，空间复杂度为$O(M + N)$。

执行用时：2ms，在所有java提交中击败了86.80%的用户。

内存消耗：40MB，在所有java提交中击败了8.33%的用户。

## 官方解题

&emsp;暂无，密切关注。