[toc]

Given a non-empty string s, you may delete **at most** one character. Judge whether you can make it a palindrome.



Note:

* The string will only contain lowercase characters `a-z`. The maximum length of the string is `50000`.



## 题目解读

&emsp;判断字符串是否可最多通过一次操作变为回文串。

```java
class Solution {
    public boolean validPalindrome(String s) {

    }
}
```

## 程序设计

* 双指针遍历查找第一个不相等字符。

```java
class Solution {
    public boolean validPalindrome(String s) {
        if (s == null || s.isEmpty()) return true;
        char[] str = s.toCharArray();

        // 找到不对称的字符
        int left = 0, right = str.length - 1;
        while (left < right && str[left] == str[right]) {
            left++;
            right--;
        }
        // 原串是回文串或者中心两个字符不相等，只需删除任意一个即可
        if (left >= right || left == right - 1) return true;
        // 删除一个，继续遍历剩余字符
        if (str[left] == str[right - 1] && isPalindrome(str, left, right - 1)) {
            return true;
        } else if (str[left + 1] == str[right] && isPalindrome(str, left + 1, right)) {
            return true;
        }
        // 存在多个不相符字符
        return false;
    }

    private boolean isPalindrome(char[] str, int left, int right) {
        while (left < right && str[left] == str[right]) {
            left++;
            right--;
        }
        return left >= right;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：8ms，在所有java提交中击败了89.47%的用户。

内存消耗：40.5MB，在所有java提交中击败了6.67%的用户。

## 官方解题

&emsp;暂无，密切关注。