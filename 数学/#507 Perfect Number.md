[toc]

A perfect number is a **positive integer** that is equal to the sum of its **positive divisors**, excluding the number itself. A **divisor** of an integer `x` is an integer that can divide `x` evenly.

Given an integer $n$, return `true` if $n$ is a perfect number, otherwise return `false`.

 

**Example 1**:

```
Input: num = 28
Output: true
Explanation: 28 = 1 + 2 + 4 + 7 + 14
1, 2, 4, 7, and 14 are all divisors of 28.
```

**Example 2**:

```
Input: num = 6
Output: true
```

**Example 3**:

```
Input: num = 496
Output: true
```

**Example 4**:

```
Input: num = 8128
Output: true
```

**Example 5**:

```
Input: num = 2
Output: false
```



**Constraints**:

* $1 \le \text{num} \le 10^8$



## 题目解读

&emsp;一个完美数字定义为该数字等于除了该数本身的正因数之和，判断一个树是否是完美数。

```java
class Solution {
    public boolean checkPerfectNumber(int num) {

    }
}
```

## 程序设计

* 以$28$为例，$28 = 2 \times 2 \times 7$，$496 = 2 \times 2 \times 2 \times 2 \times 31$，可见除了最后一个因子，都是其前因子前缀积之和。

```java
class Solution {
    public boolean checkPerfectNumber(int num) {
        if (num == 1) return false;
        int div = 2, tmp = num;
        int product = 1, sum = 1;
        // 除最后一个因子
        while (tmp > div) {
            while (tmp % div == 0) {
                product *= div;
                // 相加
                sum += product + num / product;
                tmp /= div;
            }
            div++;
        }
        return sum == num;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\sqrt{N})$，空间复杂度为$O(1)$。

执行用时：737 ms, 在所有 Java 提交中击败了10.01%的用户

内存消耗：35.2 MB, 在所有 Java 提交中击败了75.36%的用户。

## 官方解题

&emsp;官方思路采用欧几里得欧拉定理，每个偶完全数都可以写成$2^{p - 1}(2^p - 1)$的形式，$p$为素数，以上述$28$为例，$28 = 2^2(2^3 - 1)$，$496 = 2^4(2^5 - 1)$；由于数字在$10^8$的范围内，故枚举素数`2,3,5,7,13,17,19,31`即可。

```java
class Solution {
    public int pn(int p) {
        return (1 << (p - 1)) * ((1 << p) - 1);
    }
    public boolean checkPerfectNumber(int num) {
        int[] primes=new int[]{2, 3, 5, 7, 13, 17, 19, 31};
        for (int prime: primes) {
            if (pn(prime) == num) return true;
        }
        return false;
    }
}
```

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户。

内存消耗：35.5 MB, 在所有 Java 提交中击败了33.14%的用户。