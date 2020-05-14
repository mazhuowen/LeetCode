[toc]

A character in UTF8 can be from **1 to 4 bytes** long, subjected to the following rules:

* For 1-byte character, the first bit is a 0, followed by its unicode code.
* For n-bytes character, the first n-bits are all one's, the n+1 bit is 0, followed by n-1 bytes with most significant 2 bits being 10.
  This is how the UTF-8 encoding would work:

```
   Char. number range  |        UTF-8 octet sequence
      (hexadecimal)    |              (binary)
   --------------------+---------------------------------------------
   0000 0000-0000 007F | 0xxxxxxx
   0000 0080-0000 07FF | 110xxxxx 10xxxxxx
   0000 0800-0000 FFFF | 1110xxxx 10xxxxxx 10xxxxxx
   0001 0000-0010 FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx
```

Given an array of integers representing the data, return whether it is a valid utf-8 encoding.

Note:
The input is an array of integers. Only the **least significant 8 bits** of each integer is used to store the data. This means each integer represents only 1 byte of data.



## 题目解读

&emsp;`UTF-8`编码数字检测。

```java
class Solution {
    public boolean validUtf8(int[] data) {

    }
}
```

## 程序设计

* 首先需要对输入的数字截取后8位；然后判断起始数字后有几个1，并根据这个数统计其后的数字是否是`10`开头；检查完当前`UTF-8`字符，继续检查下一个`UTF-8`字符，注意输入为多个`UTF-8`编码字符，并非一个。

```java
class Solution {
    // 截取后8位
    final static int mask = 0xff;

    public boolean validUtf8(int[] data) {
        if (data == null || data.length == 0) return false;
        // 将数字取最低８位，并转换为字符串
        String[] datas = new String[data.length];
        for (int i = 0; i < data.length; i++) {
            datas[i] = Integer.toBinaryString(data[i] & mask);
        }
        // 逐个字符校验
        int start = 0;
        while (start < data.length) {
            start = validOneUtf8(datas, start);
            // start只能是小于长度len的数字表示未校验完，或者是len表示校验完
            // 如果大于len表示最后一个字符不满足要求
            if (start == -1 || start > datas.length) return false;
        }

        return true;
    }

    // 检验单个的utf-8编码，如果符合条件，返回下一个字符的起始位置
    private int validOneUtf8(String[] datas, int start) {
        String head = datas[start];
        // 最高位为0，表示单字节编码
        if (head.length() < 8) return start + 1;
        // 统计位数
        int n = 0;
        for (char c : head.toCharArray()) {
            if (c == '0') break;
            n++;
        }
        // 标识位不能是1（不能表示单字节），不能超过4个，或超出范围
        if (n == 1 || n > 4 || start + n - 1 >= datas.length) return -1;

        // 校验其后的字符串开头是否是01
        for (int i = start + 1; i < start + n; i++) {
            if (datas[i].length() < 8 || !"10".equals(datas[i].substring(0, 2))) return -1;
        }
        // 下一个起始地址
        return start + n;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：6ms，在所有java提交中击败了27.94%的用户。

内存消耗：40.3MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区采用位操作。

```java
class Solution {
        int mask = 0xff;
    
    public boolean validUtf8(int[] data) {
        // 表示当前字符的第几位
        int cnt = 0;
        for (int d : data) {
            // 截取后八位
            d &= mask;
            // 首位则截取位数
            if (cnt == 0) {
                // 两位
                if ((d >> 5) == 0b110) cnt = 1;
                // 三位
                else if ((d >> 4) == 0b1110) cnt = 2;
                // 四位
                else if ((d >> 3) == 0b11110) cnt = 3;
                // 一位，不符合要求（一位高位编码为0）
                else if ((d >> 7) == 0b1) return false;
            } else {
                // 不是首位高位必须是10
                if ((d >> 6) != 0b10) return false;
                --cnt;
            }
        }
        return cnt == 0;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了73.48%的用户。

内存消耗：40.5MB，在所有java提交中击败了100.00%的用户。