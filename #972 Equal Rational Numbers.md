[toc]

Given two strings `S` and `T`, each of which represents a non-negative rational number, return **True** if and only if they represent the same number. The strings may use parentheses to denote the repeating part of the rational number.

In general a rational number can be represented using up to three parts: an integer part, a non-repeating part, and a repeating part. The number will be represented in one of the following three ways:

* `<IntegerPart>` (e.g. 0, 12, 123)
* `<IntegerPart><.><NonRepeatingPart>`  (e.g. 0.5, 1., 2.12, 2.0001)
* `<IntegerPart><.><NonRepeatingPart><(><RepeatingPart><)>` (e.g. 0.1(6), 0.9(9), 0.00(1212))

The repeating portion of a decimal expansion is conventionally denoted within a pair of round brackets.  For example:
$$
\frac{1}{6} = 0.16666666\dots = 0.1(6) = 0.1666(6) = 0.166(66)
$$
Both 0.1(6) or 0.1666(6) or 0.166(66) are correct representations of 1 / 6.



Note:

* Each part consists only of digits.
* The `<IntegerPart>` will not begin with 2 or more zeros.  (There is no other restriction on the digits of each part.)
* $1 \le \text{<IntegerPart>.length} \le 4$
* $0 \le \text{<NonRepeatingPart>.length} \le 4$
* $1 \le \text{<RepeatingPart>.length} \le 4$



## 题目解读

&emsp;

```java
class Solution {
    public boolean isRationalEqual(String S, String T) {

    }
}
```

## 程序设计

* 

```java

```

## 性能分析

&emsp;



## 官方解题

&emsp;