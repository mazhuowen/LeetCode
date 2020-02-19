[toc]

Implement `pow(x, n)`, which calculates *x* raised to the power $n(x^n)$.



Note:

* $-100.0 < x < 100.0$
* $n$ is a 32-bit signed integer, within the range $[−2^{31}, 2^{31} − 1]$



## 题目解读

&emsp;实现平方运算，注意$n$为整形。

```java
class Solution {
    public double myPow(double x, int n) {
        
    }
}
```

## 程序设计

* 由于$n$是整形，需要注意负数极限值和正数极限值范围的不同，避免转化错误。考虑递归调用，$x^n = x^{n \% 2} + (x * x)^{n / 2}$，可以逐层分解计算。同时考虑到指数为负数尤其是负数极限值时，需要将操作数取相反数，指数取正，这一步可能导致负数正数的转换出错，由于上面$n/2$、$n\%2$都不会改变符号位，可以递归到$n$为-1时再取相反数。这样就避免了溢出。

```java
class Solution {
    public double myPow(double x, int n) {
        // 递归终止条件
        if(n == 0) {
            return 1.0D;
        }
        if(n == 1) {
            return x;
        }
        if(n == -1) {
            return 1/x;
        }
        // 递归
        return myPow(x, n % 2) * myPow(x * x, n / 2);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_2N)$，由于递归为尾调用，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：40.9MB，在所有java提交中击败了5.11%的用户。

## 官方解题

&emsp;官方也提供了递归的思路，但是整体不够巧妙，为了避免溢出采用了`long`。

```java
class Solution {
    private double fastPow(double x, long n) {
        if (n == 0) {
            return 1.0;
        }
        double half = fastPow(x, n / 2);
        if (n % 2 == 0) {
            return half * half;
        } else {
            return half * half * x;
        }
    }
    public double myPow(double x, int n) {
        long N = n;
        if (N < 0) {
            x = 1 / x;
            N = -N;
        }

        return fastPow(x, N);
    }
};
```

官方还提供了迭代思路：

```java
class Solution {
    public double myPow(double x, int n) {
        long N = n;
        if (N < 0) {
            x = 1 / x;
            N = -N;
        }
        double ans = 1;
        double current_product = x;
        for (long i = N; i > 0; i /= 2) {
            if ((i % 2) == 1) {
                ans = ans * current_product;
            }
            current_product = current_product * current_product;
        }
        return ans;
    }
};
```