[toc]

Given an integer, write an algorithm to convert it to hexadecimal. For negative integer, two’s complement method is used.

Note:

* All letters in hexadecimal (`a-f`) must be in lowercase.
* The hexadecimal string must not contain extra leading `0`s. If the number is zero, it is represented by a single zero character `'0'`; otherwise, the first character in the hexadecimal string will not be the zero character.
* The given number is guaranteed to fit within the range of a `32`-bit signed integer.
* You **must not use any method provided by the library** which converts/formats the number to hex directly.



## 题目解读

&emsp;转化整数为十六进制字符串。

```java
class Solution {
    public String toHex(int num) {

    }
}
```

## 程序设计

* 将整数的二进制形式每四位转为对应的十六进制字符。

```java
class Solution {
    char[] map = new char[]{'0', '1', '2', '3', '4', '5', '6', '7',
            '8', '9', 'a', 'b', 'c', 'd', 'e', 'f'};

    public String toHex(int num) {
        StringBuffer sb = new StringBuffer();
        // 掩码，截取4位
        int mask = 0b1111;
        while (num != 0) {
            int temp = num & mask;
            sb.insert(0, map[temp]);
            num >>>= 4;
        }
        // 对于是0的特殊情况
        if (sb.length() == 0) sb.append("0");
        return sb.toString();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.2MB，在所有java提交中击败了10.00%的用户。

## 官方解题

&emsp;暂无，密切关注。