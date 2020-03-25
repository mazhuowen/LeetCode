[toc]

Write a program to check whether a given number is an ugly number.

Ugly numbers are **positive numbers** whose prime factors only include `2, 3, 5`.



Note:

* `1` is typically treated as an ugly number.
* Input is within the 32-bit signed integer range: $[−2^{31},  2^{31} − 1]$.



## 题目解读

&emsp;判断一个数是否是2,3,5的丑数，题目注明1是一个丑数。

```java
class Solution {
    public boolean isUgly(int num) {

    }
}
```

## 程序设计

* 判断是否可被2,3,5除尽。

```java
class Solution {
    public boolean isUgly(int num) {
        // 不合法
        if (num <= 0) return false;

        while (num % 2 == 0) {
            num /= 2;
        }
        while (num % 3 == 0) {
            num /= 3;
        }
        while (num % 5 == 0) {
            num /= 5;
        }
        return num == 1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.9MB，在所有java提交中击败了5.44%的用户。

## 官方解题

&emsp;暂无，密切关注。