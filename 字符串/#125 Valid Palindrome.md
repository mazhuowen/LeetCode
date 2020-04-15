[toc]

Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

Note: For the purpose of this problem, we define empty string as valid palindrome.



## 题目解读

&emsp;判断是否是回文字符串。

```java
class Solution {
    public boolean isPalindrome(String s) {

    }
}
```

## 程序设计

* 从两侧遍历比较字符。

```java
class Solution {
    public boolean isPalindrome(String s) {
        if (s == null || s.length() <= 1) return true;

        char[] chars = s.toCharArray();
        int first = 0, second = chars.length - 1;
        while (first < second) {
            // 移动到有效的数字、字母
            while (first < second && !Character.isLetterOrDigit(chars[first])) {
                first++;
            }
            while (first < second && !Character.isLetterOrDigit(chars[second])) {
                second--;
            }
            // 比较
            if (first < second && !isSame(chars[first++], chars[second--])) return false;
        } 
        return true;
    }

    private boolean isSame(char c1, char c2) {
        if (c1 == c2) return true;
        // 大小写比较
        if (Character.isLetter(c1) && Character.isLetter(c2)) return c1 == Character.toUpperCase(c2) || Character.toUpperCase(c1) == c2;
        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了94.28%的用户。

内存消耗：40MB，在所有java提交中击败了7.14%的用户。

## 官方解题

&emsp;暂无，密切关注。