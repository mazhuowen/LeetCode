[toc]

Find the `n`th digit of the infinite integer sequence `1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ...`

Note:
$n$ is positive and will fit within the range of a 32-bit signed integer ($n < 2^{31}$).



## 题目解读

&emsp;将所有数字拼接，给出第$n$位的数字。

```java
class Solution {
    public int findNthDigit(int n) {

    }
}
```

## 程序设计

* 注意到$k$位数的数目是可计算的，$num = 10^k - 10^{k - 1}$，这样可以循环减少$n - k * num$，最后得到第$k$位数字构成的第$n$位。此时可知这个$k$位数字，可得到其中具体位的数字。

```java
class Solution {
    public int findNthDigit(int n) {
        if (n <= 0) return 0;

        int digit = 1;
        int num;
        while (n / digit > (num = digitNum(digit))) {
            n -= num * digit;
            digit++;
        }

        return digitNthNum(digit, n / digit, n % digit);
    }

    // 返回有digit位的数字数目
    private int digitNum(int digit) {
        return (int)(Math.pow(10, digit) - Math.pow(10, digit - 1));
    }

    // 返回digit位，第n数字第m位的数字
    private int digitNthNum(int digit, int a, int b) {
        // 第n个数
        int n = a + (b == 0 ? 0 : 1);
        // 第m位（从最低位0开始）
        int m = b == 0 ? 0 : (digit - b);
        // digit位的第n个数字
        int num = n - 1 + (int)Math.pow(10, digit - 1);

        // 求第n个数字的m位
        while (m > 0) {
            num /= 10;
            m--;
        }
        // 返回m位
        return num % 10;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.3MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;暂无，密切关注。