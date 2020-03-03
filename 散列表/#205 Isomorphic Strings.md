[toc]

Given two strings **s** and **t**, determine if they are isomorphic.

Two strings are isomorphic if the characters in **s** can be replaced to get **t**.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character but a character may map to itself.

**Note:**
You may assume both **s** and **t** have the same length.



## 题目解读

&emsp;给定两个字符串，判断是否是同构词。如果**s**中的字符可以被替换得到**t**，那么这两个字符串是同构的。

```java
class Solution {
    public boolean isIsomorphic(String s, String t) {

    }
}
```

## 程序设计

* 通过哈希表记录从s替换到t的映射对，特别注意的是可能存在s中不同的字符映射到t中同一字符的情况，所以还需要记录t中字符映射到s中的映射对。

```java
class Solution {
    public boolean isIsomorphic(String s, String t) {
        if(s == null || s.isEmpty() || t == null || t.isEmpty()) {
            return true;
        }
        Map<Character, Character> replaceS = new HashMap<>();
        Map<Character, Character> replaceT = new HashMap<>();
        for(int i = 0; i < s.length(); i++) {
            // 记录替换对
            if(replaceS.get(s.charAt(i)) == null && replaceT.get(t.charAt(i)) == null) {
                replaceS.put(s.charAt(i), t.charAt(i));
                replaceT.put(t.charAt(i), s.charAt(i));
            } 
            // t中的同一字符存在多个映射关系
            else if(replaceS.get(s.charAt(i)) == null && replaceT.get(t.charAt(i)) != null) {
                return false;
            }
            // 替换后不相等
            else if(replaceS.get(s.charAt(i)) != t.charAt(i)) {
                return false;
            }
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：17ms，在所有java提交中击败了31.43%的用户。

内存消耗：39MB，在所有java提交中击败了5.03%的用户。

## 官方解题

&emsp;暂无，密切关注。社区时间性能较快方法采用数组的形式保存映射。