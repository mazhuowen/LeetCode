[toc]

Find the length of the longest substring `T` of a given string (consists of lowercase letters only) such that every character in `T` appears no less than k times.



## 题目解读

&emsp;求最长子串使得子串内的字符数目不少于$k$。

```java
class Solution {
    public int longestSubstring(String s, int k) {

    }
}
```

## 程序设计

* 

```java
class Solution {

    public int longestSubstring(String s, int k) {
        if (s == null || s.length() < k) return 0;

        return longestSubstring(s.toCharArray(), 0, s.length() - 1, k);
    }

    private int longestSubstring(char[] strs, int start, int end, int k) {
        int maxLen = 0;
        if (end - start + 1 < k) return maxLen;

        int[] counter = new int[26];
        for (int i = start; i <= end; i++) counter[strs[i] - 'a']++;

        int left = start, right = start;
        while (right <= end) {
            // 当前字符不够k个
            if (counter[strs[right] - 'a'] < k) {
                maxLen = Math.max(maxLen, longestSubstring(strs, left, right - 1, k));
                left = right + 1;
            }
            right++;
        }

        if (left == start) maxLen = end - start + 1;
        else maxLen = Math.max(maxLen, longestSubstring(strs, left, end, k));
        return maxLen;
    }
}
```

## 性能分析

&emsp;

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：37.5 MB, 在所有 Java 提交中击败了25.00%的用户

## 官方解题

&emsp;