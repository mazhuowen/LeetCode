[toc]

Given a string, find the length of the longest substring T that contains at most k distinct characters.



## 题目解读

&emsp;找到包含不超过$k$个字符的最长子串长度。

```java
class Solution {
    public int lengthOfLongestSubstringKDistinct(String s, int k) {
        
    }
}
```

## 程序设计

* [#159 Longest Substring with At Most Two Distinct Characters](../散列表/#159 Longest Substring with At Most Two Distinct Characters.md)的进阶。

```java
class Solution {
    public int lengthOfLongestSubstringKDistinct(String s, int k) {
        if (s == null || s.length() == 0  || k <= 0) return 0;
        if (s.length() <= k) return s.length();

        Queue<Character> queue = new LinkedList<>();
        Map<Character, Integer> counter = new HashMap<>();
        int max = 0;

        for (char c : s.toCharArray()) {
            // 加入队列
            if (counter.get(c) != null || counter.size() < k) {
                queue.add(c);
                counter.put(c, counter.getOrDefault(c, 0) + 1);
            }
            // 需要出队
            else {
                // 更新
                max = Math.max(max, queue.size());
                 // 入队
                queue.add(c);
                counter.put(c, counter.getOrDefault(c, 0) + 1);

                // 出队，直到消除一个字符
                while (!queue.isEmpty() && counter.size() > k) {
                    char temp = queue.poll();
                    counter.put(temp, counter.get(temp) - 1);
                    if (counter.get(temp) == 0) counter.remove(temp);
                }
            }
        } 
        return Math.max(max, queue.size());
    }
}
```

* 继续优化，使用双指针代替队列。

```java
class Solution {
    public int lengthOfLongestSubstringKDistinct(String s, int k) {
        if (s == null || s.length() == 0  || k <= 0) return 0;
        if (s.length() <= k) return s.length();

        int start = 0, end = 0, max = 0;
        Map<Character, Integer> counter = new HashMap<>();

        char[] str = s.toCharArray();
        for (int i = 0; i < str.length; i++) {
            char c = str[i];
            // 加入队列
            if (counter.get(c) != null || counter.size() < k) {
                counter.put(c, counter.getOrDefault(c, 0) + 1);
                end++;
            }
            // 需要出队
            else {
                // 更新
                max = Math.max(max, end - start);
                end++;
                counter.put(c, counter.getOrDefault(c, 0) + 1);

                // 出队，直到消除一个字符
                while (start < str.length && counter.size() > k) {
                    char temp = str[start++];
                    counter.put(temp, counter.get(temp) - 1);
                    if (counter.get(temp) == 0) counter.remove(temp);
                }
            }
        } 
        return Math.max(max, end - start);
    }
}
```

## 性能分析

&emsp;队列时间复杂度为$O(N)$，最坏情况为$O(KN)$，空间复杂度为$O(N)$。

执行用时：11ms，在所有java提交中击败了60.87%的用户。

内存消耗：41MB，在所有java提交中击败了100.00%的用户。

&emsp;优化后时间复杂度，时间复杂度不变。

执行用时：10ms，在所有java提交中击败了66.50%的用户。

内存消耗：40.1MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方提供了时间复杂度稳定为$O(N)$的方法，使用字典保存每个字符出现的最后的位置。