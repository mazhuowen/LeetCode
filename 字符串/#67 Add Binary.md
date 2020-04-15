[toc]

Given two binary strings, return their sum (also a binary string).

The input strings are both **non-empty** and contains only characters `1` or `0`.



Constraints:

* Each string consists only of `'0'` or `'1'` characters.
* $1 \le \text{a.length, b.length} \le 10^4$
* Each string is either `"0"` or doesn't contain any leading zero.



## 题目解读

&emsp;字符串二进制加法。

```java
class Solution {
    public String addBinary(String a, String b) {
        
    }
}
```

## 程序设计

* 需注意字符串高位是数字的低位。

```java
class Solution {
    public String addBinary(String a, String b) {
        if (a == null || b == null || a.length() < 1 || b.length() < 1) throw new IllegalArgumentException("invalid param");
        int n = a.length() - 1, m = b.length() - 1;
        StringBuffer sb = new StringBuffer();

        // 保存进位
        int r = 0;
        while (n >= 0 && m >= 0) {
            int num = (a.charAt(n--) - '0') + (b.charAt(m--) - '0') + r;
            r = addSingleNum(num, sb);
        }
        while (n >= 0) {
            int num = (a.charAt(n--) - '0') + r;
            r = addSingleNum(num, sb);
        }

        while (m >= 0) {
            int num = (b.charAt(m--) - '0') + r;
            r = addSingleNum(num, sb);
        }
        if (r != 0) sb.insert(0, "1");
        return sb.toString();
    }

    // 返回进位
    private int addSingleNum(int res, StringBuffer sb) {
        switch(res) {
            case 0:
                sb.insert(0, "0");
                return 0;
            case 1:
                sb.insert(0, "1");
                return 0;
            case 2:
                sb.insert(0, "0");
                return 1;
            case 3:
                sb.insert(0, "1");
                return 1;
            default:
                throw new IllegalArgumentException("invalid param");
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\max(N,M)$，空间复杂度为$O(\max(N,M))$。

执行用时：2ms，在所有java提交中击败了98.04%的用户。

内存消耗：38.1MB，在所有java提交中击败了7.69%的用户。

## 官方解题

&emsp;还提供了基于位操作的解法。