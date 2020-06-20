[toc]

Let's say a positive integer is a superpalindrome if it is a palindrome, and it is also the square of a palindrome.

Now, given two positive integers `L` and `R` (represented as strings), return the number of superpalindromes in the inclusive range `[L, R]`.

Note:

* $1 \le \text{len(L)} \le 18$
* $1 \le \text{len(R)} \le 18$
* `L` and `R` are strings representing integers in the range $[1, 10^{18})$.
* $\text{int(L)} \le \text{int(R)}$



## 题目解读

&emsp;统计给定区间的超级回文数，超级回文数定义是由回文数字的平方值，且数值也是回文形式。

```java
class Solution {
    public int superpalindromesInRange(String L, String R) {

    }
}
```

## 程序设计

* 遍历开方数的前半部分，然后判断乘积是否是回文。

```java
class Solution {

    public int superpalindromesInRange(String L, String R) {
        long left = Long.parseLong(L), right = Long.parseLong(R);

        int count = 0;
        int limit = 99999;
        for (int i = 1; i <= limit; i++) {
            // 将i作为前半部分
            StringBuffer temp = new StringBuffer(Integer.toString(i));
            StringBuffer sb = new StringBuffer(temp).append(temp.reverse());

            // 偶数长度
            long even = Long.parseLong(sb.toString());
            even *= even;
            // 奇数长度
            sb.deleteCharAt(sb.length() / 2);
            long odd = Long.parseLong(sb.toString());
            odd *= odd;

            if (even > right && odd > right) break;
            if (even >= left && even <= right && isPalindrome(even)) count++;
            if (odd >= left && odd <= right && isPalindrome(odd)) count++;
        }

        return count;
    }

    private boolean isPalindrome(long num) {
        long temp = 0;
        while (num > temp) {
            temp = temp * 10 + num % 10;
            num /= 10;
        }
        return num == temp || num == temp / 10;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\sqrt[4]{N} + \log_2N)$，空间复杂度为$O(\log_2N)$用于保存拼接字符串。

执行用时：456ms，在所有java提交中击败了24.19%的用户。

内存消耗：40.1MB，在所有java提交中击败了66.67%的用户。

## 官方解题

&emsp;官方同上。社区时间性能较好的方法观察得到在题目限定范围内的超级回文数的开方值都是由`0`、`1`、`2`构成，从而直接回溯遍历。不具有一般性。