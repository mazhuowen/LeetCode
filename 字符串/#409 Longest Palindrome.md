[toc]

Given a string which consists of lowercase or uppercase letters, find the length of the longest palindromes that can be built with those letters.

This is case sensitive, for example `"Aa"` is not considered a palindrome here.

Note:
Assume the length of given string will not exceed 1,010.



## 题目解读

&emsp;统计给定字符串字符可构成的最长回文串。

```java
class Solution {
    public int longestPalindrome(String s) {

    }
}
```

## 程序设计

* 最基本的思路是统计字符数，最长回文串就是每个字符的最大偶数数目叠加，如果存在奇数，最后还要加一。

```java
class Solution {
    public int longestPalindrome(String s) {
        if (s == null || s.length() == 0) return 0;
        // 计数，注意索引
        int[] counter = new int[52];
        for (char c : s.toCharArray()) {
            int idx = c - 'A' >= 26 ? (c - 'a' + 26) : (c - 'A');
            counter[idx]++;
        }

        // 统计
        int sum = 0;
        boolean odd = false;
        for (int count : counter) {
            if (count % 2 == 0) sum += count;
            else {
                sum += count - 1;
                odd = true;
            }
        }
        return sum + (odd ? 1 : 0);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了75.00%的用户。

内存消耗：37.7MB，在所有java提交中击败了5.00%的用户。

## 官方解题

&emsp;官方解题思路一致。