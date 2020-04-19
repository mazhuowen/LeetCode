[toc]

Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000.



## 题目解读

&emsp;找到字符串中的最长回文子串。

```java
class Solution {
    public String longestPalindrome(String s) {

    }
}
```

## 程序设计

* 最基本的思路是暴力遍历：

```java
class Solution {
    public String longestPalindrome(String s) {
        String max = "";
        if (s == null || s.isEmpty()) return max;
        
        char[] chars = s.toCharArray();
        // 暴力匹配回文字符串起始结尾索引
        for (int i = 0; i < chars.length; i++) {
            for (int j = chars.length - 1; j >= i; j--) {
                // 以i开头的最长回文串，匹配到则比较更新最大回文串
                if (isPalindrome(chars, i, j)) {
                    if (j - i + 1 > max.length()) max = s.substring(i, j + 1);
                    // 当前最大回文串已找到，跳过后续遍历
                    break;
                }
            }
        }
        return max;
    }

    private boolean isPalindrome(char[] s, int i, int j) {
        while (i < j) {
            if (s[i++] != s[j--]) return false;
        }
        return true;
    }
}
```

* 上述暴力法需要三层循环，实际上回文字符串由中心向左右两侧扩散，可以遍历中心字符，然后向左向右遍历比较，这样只需要两个循环。

```java
class Solution {
    public String longestPalindrome(String s) {
        String max = "";
        if (s == null || s.isEmpty()) return max;

        for (int i = 0; i < s.length(); i++) {
            // 当前字符为中心次，扩展
            max = findMaxSeq(s, i - 1, i + 1, max);
            // 当前字符和其后字符为中心次，扩展
            if (i + 1 < s.length() && s.charAt(i) == s.charAt(i + 1)) max = findMaxSeq(s, i - 1, i + 2, max);
        }
        return max;
    }

    // 扩展
    private String findMaxSeq(String s, int left, int right, String max) {
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        } 
        if (right - left - 1 > max.length()) max = s.substring(left + 1, right);
        return max;
    }
}
```

> String可转化为字符数组，会加快时间性能。

## 性能分析

&emsp;暴力匹配时间复杂度为$O(N^3)$，空间复杂度为$O(N)$。

执行用时：167ms，在所有java提交中击败了25.07%的用户。

内存消耗：38MB，在所有java提交中击败了25.89%的用户。

&emsp;根据中心字符扩展的时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：37ms，在所有java提交中击败了66.58%的用户。

内存消耗：39.8MB，在所有java提交中击败了15.18%的用户。

## 官方解题

&emsp;官方还提供了动态规划的思路。