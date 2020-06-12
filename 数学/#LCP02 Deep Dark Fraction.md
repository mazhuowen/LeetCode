[toc]

化简连分数为最简分数，连分数形式为
$$
a_0 + \frac{1}{a_1 + \frac{1}{a_2 + \frac{1}{\cdots}}}
$$


限制：

* $\text{cont[i]} \ge 0$
* $1 \le \text{cont的长度} \le 10$
* `cont`最后一个元素不等于$0$
* 答案的$n$, $m$的取值都能被$32$位int整型存下（即不超过$2^{31} - 1$）。



## 题目解读

&emsp;分数化简。

```java
class Solution {
    public int[] fraction(int[] cont) {

    }
}
```

## 程序设计

* 模拟计算，从尾到头遍历计算。

```java
class Solution {
    public int[] fraction(int[] cont) {
        if (cont == null || cont.length == 0) throw new IllegalArgumentException("invalid param");

        Fraction a = null, b = new Fraction(1, cont[cont.length - 1]);
        for (int i = cont.length - 2; i >= 0; i--) {
            a = new Fraction(cont[i], 1);
            b = sumAndTran(a, b);
        }
        // 最后计算结果为b，由于计算时候颠倒过来了，此处需要换下位置
        return new int[]{(int)b.d, (int)b.n};
    }

    // 向加并颠倒
    private Fraction sumAndTran(Fraction a, Fraction b) {
        long n = a.n * b.d + a.d * b.n;
        long d = a.d * b.d;
        // 颠倒位置
        return new Fraction(d, n);
    }
}

class Fraction {
    long n;
    long d;

    Fraction(long n, long d) {
        long gcd = gcd(n, d);
        this.n = n / gcd;
        this.d = d / gcd;
    }

    private static long gcd(long x, long y) {
        if (x == 0) return y;
        return gcd(y % x, x);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.6MB，在所有java提交中击败了33.33%的用户。

## 官方解题

&emsp;暂无，密切关注。实际上上述式子已经是极简分数，最底层为$a_{n - 1} + \frac{1}{a_n}$，其组合结果明显不可约；往上类推，假设相加的数为$a_{i} + \frac{y}{x}$，其中$y,x$不可约，明显$\frac{a_ix + y}{x}$也不可约。这样形式可以简化为：

```java
class Solution {
    public int[] fraction(int[] cont) {
        if (cont == null || cont.length == 0) throw new IllegalArgumentException("invalid param");

        int n = 1, d = cont[cont.length - 1];
        for (int i = cont.length - 2; i >= 0; i--) {
            int temp = d;
            d = d * cont[i] + n;
            n = temp;
        }
        return new int[]{d, n};
    }
}
```