[toc]

Given two strings **s** and **t** , write a function to determine if **t** is an anagram of **s**.

Note:
You may assume the string contains only lowercase alphabets.

Follow up:
What if the inputs contain unicode characters? How would you adapt your solution to such case?



## 题目解读

&emsp;给定两个字符串**s**和**t**，编写一个函数来判断**t**是否是**s** 的字母异位词。

```java
class Solution {
    public boolean isAnagram(String s, String t) {

    }
}
```

## 程序设计

* 最简单的想法是统计字符串s中的字符计数，然后遍历字符串t，减去计数，如果出现负数说明不符合要求。

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if(s == null && t == null || s.isEmpty() && t.isEmpty()) {
            return true;
        }
        if(s.length() != t.length()) {
            return false;
        }
        // s中的字符计数
        Map<Character, Integer> record = new HashMap<>();
        for(char c : s.toCharArray()) {
            record.put(c, record.getOrDefault(c, 0) + 1);
        }
        // 遍历t，减去计数
        for(char c : t.toCharArray()) {
            record.put(c, record.getOrDefault(c, 0) - 1);
            // 计数为负，不符合要求
            if(record.get(c) < 0) {
                return false;
            }
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：17ms，在所有java提交中击败了23.11%的用户。

内存消耗：42.1MB，在所有java提交中击败了5.01%的用户。

## 官方解题

&emsp;除了上述思路，官方还提供了排序字符，然后逐个比较的方法。社区中时间性能较好的方法只不过把字典映射用数组实现。