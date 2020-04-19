[toc]

Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.

**Follow up:**

Coud you solve it without converting the integer to a string?



## 题目解读

&emsp;判断数字是否是回文数字。

```java
class Solution {
    public boolean isPalindrome(int x) {
        
    }
}
```

## 程序设计

* 最简单的思路是转换为字符串，然后翻转并比较。但题目要求不能直接转换，可采用数字翻转比较的形式来解决。

```java
class Solution {
    public boolean isPalindrome(int x) {
        int temp = x;
        // 翻转
        int reverse = 0;
        while (temp > 0) {
            reverse = reverse * 10 + (temp % 10);
            temp /= 10;
        }
        return reverse == x;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：9ms，在所有java提交中击败了99.11%的用户。

内存消耗：38.8MB，在所有java提交中击败了5.14%的用户。

## 官方解题

&emsp;官方做了进一步优化，只翻转一半。

```java
class Solution {
    public boolean isPalindrome(int x) {
        // 最低位为0，不是回文
        if (x != 0 && x % 10 == 0) return false;

        int temp = x;
        int reverse = 0;
        // 翻转一半
        while (temp > reverse) {
            reverse = reverse * 10 + (temp % 10);
            temp /= 10;
        }
        // 正对奇数位情况和偶数位情况做判断
        return reverse == temp || reverse / 10 == temp;
    }
}
```

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：9ms，在所有java提交中击败了99.11%的用户。

内存消耗：38.8MB，在所有java提交中击败了5.14%的用户。