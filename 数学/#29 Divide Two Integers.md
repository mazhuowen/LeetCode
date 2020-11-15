[toc]

Given two integers `dividend` and `divisor`, divide two integers without using multiplication, division and mod operator.

Return the quotient after dividing `dividend` by `divisor`.

The integer division should truncate toward zero.



**Note**:

* Both dividend and divisor will be 32-bit signed integers.
* The divisor will never be 0.
* Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: `[−2^31,  2^31 − 1]`. For the purpose of this problem, assume that your function returns `2^31 − 1` when the division result overflows.



## 题目解读

&emsp;给定整数除数和被除数，不利用乘、除和取余操作的情况下得到结果。

```java
class Solution {
    public int divide(int dividend, int divisor) {
        
    }
}
```

## 程序设计

* 在不使用乘除等操作的情况下，可通过移位来实现。考虑$10 / 3$，通过对除数移位逼近，得到$6$最接近$10$，记录此时的商$2$，对残差$4$继续拟合，得到商为$1$，加入此前的结果，得到$3$；对残差$1$继续拟合，商为$0$。最后得到结果$3$。
* 有了上述思路需要考虑题目限定运行存储为整形32位，此时需要注意溢出的问题。负数极限值为`-2147483648`，正数极限值为`2147483647`，显然不能将负数转换为整数再做处理。鉴于绝对值负数比正数范围更大，可以将两个数转化为负数进行操作。

```java
class Solution {
    public int divide(int dividend, int divisor) {
        if (dividend == 0) return 0;
        // 溢出情况处理
        if (dividend == Integer.MIN_VALUE && divisor == -1) {
            return Integer.MAX_VALUE;
        }
        // 结果标识
        boolean flag = (dividend ^ divisor) < 0;

        // 转化为负数（避免溢出）
        dividend = dividend < 0 ? dividend : -dividend;
        divisor = divisor < 0 ? divisor : -divisor;

        int result = 0;
        // 即转化前分母比分子小，结束循环（即残差商为0）
       while (dividend <= divisor) {
           int temp = divisor;
           int count = 0;
           // 由于是负数，移位直到temp小于等于dividend。需注意特殊情况dividend为-2147483648，此时temp>=dividend会造成死循环，因为当temp=dividend时，会继续迭代移位，得到的下一个值就是0，0肯定大于任意负数，从而造成死循环，所以要加temp<0的判断。这样最后得到的count就是移位后使得temp >= dividend的值，count-1就是满足最逼近dividend的移位
           while (temp >= dividend && temp < 0) {
               temp <<= 1;
               count++;
           }
           // 记录结果
           result += 1 << (count - 1);
           // 减去参差，继续迭代拟合
           dividend -= (divisor << (count - 1));
       }
        // 加入符号
        return flag ? -result : result;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_2N)$，其中$N$为二进制表示长度。空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.5MB，在所有java提交中击败了5.03%的用户。

## 官方解题

&emsp;暂无，密切关注。