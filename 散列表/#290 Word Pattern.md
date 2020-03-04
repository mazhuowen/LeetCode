[toc]

Given a `pattern` and a string `str`, find if `str` follows the same pattern.

Here follow means a full match, such that there is a bijection between a letter in `pattern` and a non-empty word in `str`.

Notes:
You may assume `pattern` contains only lowercase letters, and `str` contains lowercase letters that may be separated by a single space.



## 题目解读

&emsp;给定模式和字符串，判断是否符合。

```java
class Solution {
    public boolean wordPattern(String pattern, String str) {

    }
}
```

## 程序设计

* 哈希表记录模式字符和单词对应关系即可。

```java
class Solution {
    public boolean wordPattern(String pattern, String str) {
        if(pattern == null || pattern.isEmpty()) {
            return str == null || str.isBlank();
        }
        String[] words = str.split(" ");
        if(pattern.length() != words.length) {
            return false;
        }
        // 记录模式到单词
        Map<Character, String> counter1 = new HashMap<>();
        // 记录单词到模式
        Map<String, Character> counter2 = new HashMap<>();
        for(int i = 0; i < words.length; i++) {
            // 第一次遇到该模式，加入
            if(counter1.get(pattern.charAt(i)) == null && counter2.get(words[i]) == null) {
                counter1.put(pattern.charAt(i), words[i]);
                counter2.put(words[i], pattern.charAt(i));
            } 
            // 判断映射是否对应
            else if(counter1.get(pattern.charAt(i)) != null && counter2.get(words[i]) != null) {
                if(!counter1.get(pattern.charAt(i)).equals(words[i])) {
                    return false;
                }
                if(counter2.get(words[i]) != pattern.charAt(i)) {
                    return false;
                }
            } 
            // 两者有一个为空，表示映射关系不唯一
            else {
                return false;
            }
        }
        return true;
    }   
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了98.80%的用户。

内存消耗：37.8MB，在所有java提交中击败了5.15%的用户。

## 官方解题

&emsp;暂无，密切关注。