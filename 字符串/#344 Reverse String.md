[toc]

Write a function that reverses a string. The input string is given as an array of characters `char[]`.

Do not allocate extra space for another array, you must do this by **modifying the input array** in-place with $O(1)$ extra memory.

You may assume all the characters consist of printable ascii characters.



## 题目解读

&emsp;翻转字符串。

```java
class Solution {
    public void reverseString(char[] s) {

    }
}
```

## 程序设计

* 交换位置。

```java
class Solution {
    public void reverseString(char[] s) {
        int i= 0, j = s.length - 1;

        while (i < j) {
            char temp = s[i];
            s[i++] = s[j];
            s[j--] = temp;
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了99.99%的用户。

内存消耗：46.4MB，在所有java提交中击败了94.55%的用户。

## 官方解题

&emsp;同上。