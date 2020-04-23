[toc]

Given two non-negative integers `num1` and `num2` represented as string, return the sum of `num1` and `num2`.



Note:

* The length of both `num1` and `num2` is < 5100.
* Both `num1` and `num2` contains only digits `0-9`.
* Both `num1` and `num2` does not contain any leading zero.
* You **must not use any built-in BigInteger library** or **convert the inputs to integer** directly.



## 题目解读

&emsp;字符串加法。

```java
class Solution {
    public String addStrings(String num1, String num2) {

    }
}
```

## 程序设计

* 字符串加法，注意最后进位判断。

```java
class Solution {
    public String addStrings(String num1, String num2) {
        if (num1 == null || num2 == null || num1.isEmpty() || num2.isEmpty()) throw new IllegalArgumentException("invalid param");

        if ("0".equals(num1) || "0".equals(num2)) return "0".equals(num1) ? num2 : num1;

        int carry = 0, sum;
        int m = num1.length(), n = num2.length();
        char[] res = new char[Math.max(m, n) + 1];
        int idx = 0;

        while (idx < m && idx < n) {
            sum = transToNum(num1.charAt(m - idx - 1)) + transToNum(num2.charAt(n - idx - 1)) + carry;

            res[res.length - idx - 1] = (char)(sum % 10 + '0');
            carry = sum / 10;
            idx++;
        }

        while (idx < m) {
             sum = transToNum(num1.charAt(m - idx - 1)) + carry;

            res[res.length - idx - 1] = (char)(sum % 10 + '0');
            carry = sum / 10;
            idx++;
        }

        while (idx < n) {
             sum = transToNum(num2.charAt(n - idx - 1)) + carry;

            res[res.length - idx - 1] = (char)(sum % 10 + '0');
            carry = sum / 10;
            idx++;
        }
        if (carry > 0) {
            res[0] = (char)(carry + '0');
            return new String(res);
        }
        else return new String(res, 1, res.length - 1);
    }

    private int transToNum(char c) {
        return c - '0';
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\max(M,N))$，空间复杂度为$O(\max(M,N))$。

执行用时：2ms，在所有java提交中击败了99.34%的用户。

内存消耗：39.7MB，在所有java提交中击败了8.33%的用户。

## 官方解题

&emsp;暂无，密切关注。