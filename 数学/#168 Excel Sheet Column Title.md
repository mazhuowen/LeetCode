[toc]

Given a positive integer, return its corresponding column title as appear in an Excel sheet.



## 题目解读

&emsp;$10$进制转$26$进制。

```java
class Solution {
    public String convertToTitle(int n) {

    }
}
```

## 程序设计

* 在$26$进制字符串转$10$进制时，每次都需要乘$26$并加上字符值并加$1$，`num1 = num2 * 26 + c - 'A' + 1`，由于`c - 'A'`在$26$的范围内，再加上$1$可能会超过$26$，从而在得到`num2`时可能会实际值大一，故应该先减去$1$，再取余并除$26$。

```java
class Solution {
    public String convertToTitle(int n) {
        if (n <= 0) throw new IllegalArgumentException("invalid param"); 
        StringBuffer sb = new StringBuffer();

        while (n > 0) {
            // 关键逻辑
            n--;
            sb.insert(0, (char)(n % 26 + 'A'));
            n /= 26;
        }
        return sb.toString();
    }
}
```

## 性能分析

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.8MB，在所有java提交中击败了14.29%的用户。

## 官方解题

&emsp;暂无，密切关注。