[toc]

Implement `atoi` which converts a string to an integer.

The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned.



Note:

* Only the space character `' '` is considered as whitespace character.
* Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: $[−2^{31},  2^{31} − 1]$. If the numerical value is out of the range of representable values, INT_MAX ($2^{31} − 1$) or INT_MIN ($−2^{31}$) is returned.



## 题目解读

&emsp;给定字符串，判定第一个非空字符及之后字符是否是数字，并返回。

```java
class Solution {
    public int myAtoi(String str) {

    }
}
```

## 程序设计

* 题目规定数字如果存在必须是第一个非空字符，首先需要查找第一个非空字符，如果不是符号或数字，则不存在；数字需要注意溢出的问题，负数和正数的边界不一样。

```java
class Solution {
    public int myAtoi(String str) {
        if (str == null || str.length() == 0) return 0;

        // 记录符号
        boolean flag = false;
        char[] strs = str.toCharArray();
        // 遍历查找第一个非空字符
        int idx = 0;
        while (idx < strs.length && strs[idx] == ' ') {
            idx++;
        }
        // 不存在数字或符号
        if (idx == strs.length) return 0;
        
        // 判断符号并移动到数字
        if (strs[idx] == '-' || strs[idx] == '+') {
            flag = strs[idx++] == '-';
        }

        // 符号位后面不是数字
        if (idx >= strs.length || !Character.isDigit(strs[idx])) return 0;
       
        // 为了防止数字溢出，使用长整形
        long num = 0;
        while(idx < strs.length && Character.isDigit(strs[idx]) && num - 1 < Integer.MAX_VALUE) {
            num = num * 10 + (strs[idx++] - '0');
        }
        // 正负数的处理
        if (flag) {
            return num - 1 >= Integer.MAX_VALUE ? Integer.MIN_VALUE : (int)-num;
        } else {
            return num >= Integer.MAX_VALUE ? Integer.MAX_VALUE : (int)num;
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了98.31%的用户。

内存消耗：39.6MB，在所有java提交中击败了5.77%的用户。

## 官方解题

&emsp;官方巧妙的使用有限状态机的思路来求解。

<img src="../images/#8.png" style="zoom:67%;" />

​			

|           | ' '   | +/-    | number    | other |
| --------- | ----- | ------ | --------- | ----- |
| start     | start | signed | in_number | end   |
| signed    | end   | end    | in_number | end   |
| in_number | end   | end    | in_number | end   |
| end       | end   | end    | end       | end   |