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



**Note**:

* Each part consists only of digits.
* The `<IntegerPart>` will not begin with 2 or more zeros.  (There is no other restriction on the digits of each part.)
* $1 \le \text{<IntegerPart>.length} \le 4$
* $0 \le \text{<NonRepeatingPart>.length} \le 4$
* $1 \le \text{<RepeatingPart>.length} \le 4$



## 题目解读

&emsp;比较两个有理数是否相等。

```java
class Solution {
    public boolean isRationalEqual(String S, String T) {

    }
}
```

## 程序设计

* 主要难点在于有理数的表达形式，任何有理数都可以用分数形式表达；对于`aaa.bb(cc)`的形式，可以分别转化为分数形式；
  * 整数部分`aaa`就是分子为`aaa`，分母为$1$的分数；
  * 固定小数部分`0.bb`就是分子为`bb`，分母为$10^{len}$的分数；
  * 关键的，对于重复小数部分`cc`，可以看做是`0.00cc`、`0.0000cc`、……的叠加，可表示为$\frac{val}{10^{len1}}(\frac{1}{10^{len2}} + \frac{1}{10^{2*len2}} + \cdots + \frac{1}{10^\infty})$，令$r = \frac{1}{10^{len2}}$，根据等比数列求和公式，可得$\frac{val}{10^{len1}}(r + r^2 + \cdots) = \frac{val}{10^{len1}}(\frac{r}{1 - r}) = \frac{val}{10^{len1}(10^{len2} - 1)}$，其中$len1$为固定小数长度，$len2$为重复小数长度。
* 将上述三个分数相加就得到了有理数的分数形式；通过分子分母化简约分，可得到最终形式。两个有理数的比较转化为分数形式分子分母的比较。

```java
class Solution {
    public boolean isRationalEqual(String S, String T) {
        Fraction num1 = trans2frac(S);
        Fraction num2 = trans2frac(T);
        return num1.n == num2.n && num1.d == num2.d;
    }

    private Fraction trans2frac(String num) {
        String[] nums = num.split("[.()]");
        int len = nums.length;

        Fraction base = null;
        // 整数部分不为0
        if (0 < len && nums[0] != null && !nums[0].isEmpty()) base = new Fraction(Long.valueOf(nums[0]), 1);

        // 固定不重复小数部分
        int len1 = 0;
        if (1 < len && nums[1] != null && !nums[1].isEmpty()) {
            len1 = nums[1].length();
            if (base == null) base = new Fraction(Long.valueOf(nums[1]), (long)Math.pow(10, len1));
            else base = base.add(new Fraction(Long.valueOf(nums[1]), (long)Math.pow(10, len1)));
        }

        // 重复小数部分
        int len2 = 0;
        if (2 < len && nums[2] != null && !nums[2].isEmpty()) {
            len2 = nums[2].length();
            long div = ((long)Math.pow(10, len2) - 1) * (long)Math.pow(10, len1);
            if (base == null) base = new Fraction(Long.valueOf(nums[2]), div);
            else base = base.add(new Fraction(Long.valueOf(nums[2]), div));
        }
        if (base == null) throw new IllegalArgumentException("invalid param");
        return base;
    }
}

class Fraction {
    // 分子
    long n;
    // 分母
    long d;

    static Calculation helper = new Calculation();

    // 化简保存为分子分母形式
    Fraction(long n, long d) {
        long gcd = helper.gcd(n, d);
        this.n = n / gcd;
        this.d = d / gcd;
    }

    // 分数相加
    public Fraction add(Fraction other) {
        long newN = this.n * other.d + this.d * other.n;
        long newD = this.d * other.d;
        return new Fraction(newN, newD);
    }
}

class Calculation {

    public long gcd(long x, long y) {
        if (x == 0) return y;
        return gcd(y % x, x);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：4ms，在所有java提交中击败了70.27%的用户。

内存消耗：38.1MB，在所有java提交中击败了20.00%的用户。

## 官方解题

&emsp;上述思路参考官方解题。