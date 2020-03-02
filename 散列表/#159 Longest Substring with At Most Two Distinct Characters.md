[toc]

Given a string **s** , find the length of the longest substring **t**  that contains **at most** 2 distinct characters.



## 题目解读

&emsp;给定一个字符串**s** ，找出 至多包含两个不同字符的最长子串**t**。

```java
class Solution {
    public int lengthOfLongestSubstringTwoDistinct(String s) {

    }
}
```

## 程序设计

* 仍然是队列的问题，需要字典记录当前序列的字符和字符数目，根据题目要求可知不能超过2个，每次入队需要判断序列字符是否超过2个，超过则需要出队并减少计数，直到有一个字符计数为0，此时队列字符又达到了2个，统计序列长度并更新。

```java
class Solution {
    public int lengthOfLongestSubstringTwoDistinct(String s) {
        if(s == null || s.isEmpty()) {
            return 0;
        }
        // 记录最大序列长度
        int maxLen = 0;
        // 计数
        Map<Character, Integer> record = new HashMap<>();
        // 队列首尾
        int start = 0, end = 0;
        while(end < s.length()) {
            // 入队
            record.put(s.charAt(end), record.getOrDefault(s.charAt(end++), 0) + 1);
            // 序列已达最大限制，出队队首值，直到序列小于等于2
            while(record.size() > 2) {
                int temp = record.getOrDefault(s.charAt(start), 0);
                // 减一后计数为0，溢出键值
                if(temp == 1) {
                    record.remove(s.charAt(start));
                } 
                // 减一
                else {
                    record.put(s.charAt(start), temp - 1);
                }
                // 出队
                start++;
            }
            // 根据当前序列长度更新最大值
            maxLen = Math.max(maxLen, end - start);
        }
        return maxLen;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：6ms，在所有java提交中击败了80.04%的用户。

内存消耗：39.2MB，在所有java提交中击败了5.26%的用户。

## 官方解题

&emsp;同上。