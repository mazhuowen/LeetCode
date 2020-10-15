[toc]

Given a string representing an expression of fraction addition and subtraction, you need to return the calculation result in string format. The final result should be `irreducible fraction`. If your final result is an integer, say $2$, you need to change it to the format of fraction that has denominator $1$. So in this case, $2$ should be converted to $2/1$.



**Note**:

* The input string only contains `'0'` to `'9'`, `'/'`, `'+'` and `'-'`. So does the output.
* Each fraction (input and output) has format `±numerator/denominator`. If the first input fraction or the output is positive, then `'+'` will be omitted.
* The input only contains valid **irreducible fractions**, where the **numerator** and **denominator** of each fraction will always be in the range $[1,10]$. If the denominator is $1$, it means this fraction is actually an integer in a fraction format defined above.
* The number of given fractions will be in the range $[1,10]$.
* The numerator and denominator of the **final result** are guaranteed to be valid and in the range of 32-bit int.



## 题目解读

&emsp;计算分数表达式。

```java
class Solution {
    public String fractionAddition(String expression) {

    }
}
```

## 程序设计

* 构建分数类，对分子分母进行约分。

```java
class Solution {
    public String fractionAddition(String expression) {
        Fraction res = new Fraction(0, 1);
        int idx = 0;
        // 表示当前是分子还是分母，是否是加法
        boolean isN = true, isA = true;
        int tmp = 0;
        while (idx < expression.length()) {
            char c = expression.charAt(idx);
            if (c == '-' || c == '+') {
                isN = true;
                isA = c == '+';
                idx++;
            } else if (c == '/') {
                isN = false;
                idx++;
            } else {
                int num = 0;
                while (idx < expression.length() && Character.isDigit(expression.charAt(idx))) {
                    num = num * 10 + (expression.charAt(idx++) - '0');
                }
                // 是分子，则保存
                if (isN) tmp = num;
                // 是分母，计算分数
                else res = isA ? res.add(new Fraction(tmp, num)) : res.mul(new Fraction(tmp, num));
            }
        }
        return res.toString();
    }
}

class Fraction {
    int numerator;
    int denominator;

    Fraction(int n, int d) {
        this.numerator = n;
        this.denominator = d;
        // 消去公因子
        if (n != 0 && d != 0) {
            int gcd = gcd(n, d);
            this.numerator /= gcd;
            this.denominator /= gcd;
        }
    }

    private int gcd(int x, int y) {
        if (x == 0) return y;
        return gcd(y % x, x);
    }

    public Fraction add(Fraction that) {
        return new Fraction(this.numerator * that.denominator + that.numerator * this.denominator, this.denominator * that.denominator);
    }

    public Fraction mul(Fraction that) {
        return new Fraction(this.numerator * that.denominator - that.numerator * this.denominator, this.denominator * that.denominator);
    }

    public String toString() {
        if (this.numerator == 0) return "0/1";
        // 分母小于0，进行转换
        if (this.denominator < 0) {
            this.denominator *= -1;
            this.numerator *= -1;
        }
        return this.numerator + "/" + this.denominator;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：5ms，在所有java提交中击败了88.77%的用户。

内存消耗：36.9MB，在所有java提交中击败了91.41%的用户。

## 官方解题

&emsp;官方思路类似。