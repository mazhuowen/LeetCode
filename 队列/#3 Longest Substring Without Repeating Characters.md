[toc]

Given a string, find the length of the longest substring without repeating characters.



## 题目解读

&emsp;给定字符串，找到最大的不重复子串。

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
    
    }
}
```

## 程序设计

* 最简单的使用队列来模拟，如果当前队列不存在待入队值，则直接入队；如果存在重复值，则出队。

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if(s == null || s.isEmpty()) {
            return 0;
        }
        int maxLen = 0;
        // 队列
        Queue<Character> queue = new LinkedList<>();
        for(char c : s.toCharArray()) {
            // 如果队列中已存在，则出队，直到不存在
            while(!queue.isEmpty() && queue.contains(c)) {
                queue.poll();
            }
            queue.add(c);
            maxLen = Math.max(maxLen, queue.size());
        }
        return maxLen;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：28ms，在所有java提交中击败了22.25%的用户。

内存消耗：41.4MB，在所有java提交中击败了5.01%的用户。

## 官方解题

&emsp;官方思路一致，只是使用了双指针和集合来模拟出队入队操作。