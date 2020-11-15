[toc]

Given a positive integer $N$, how many ways can we write it as a sum of consecutive positive integers?



**Note:** 

$1 \le N \le 10 ^ 9$.



## 题目解读

&emsp;判断数字由连续数字之和构成的数目。

```java
class Solution {
    public int consecutiveNumbersSum(int N) {

    }
}
```

## 程序设计

* 仔细分析规律，由于是连续数字，必然是等差序列，假设起始数字为$a$，序列长度为$l$，则满足$\frac{a + a + l - 1}{2} \times l = num$，可得$a = \frac{2num - l^2 + l}{2l}$。
* 根据上式，可以遍历尝试长度$l$，判断计算得到的$a$是否是整数；其次最长的序列长素不超过从$1$开始，和小于等于$N$的长度，即$\frac{l + 1}{2} \times l \le num \implies l \le \sqrt{2 * N + 0.25} - 0.5$。

```java
class Solution {
    public int consecutiveNumbersSum(int N) {
        if (N < 1) throw new IllegalArgumentException("invalid param");

        int count = 0;
        int maxLen = (int)(Math.sqrt(2 * N + 0.25) - 0.5);

        for (int len = 1; len <= maxLen; len++) {
            int val = 2 * N - len * len + len;
            if (val < 1 || val % (2 * len) != 0) continue;
            count++;
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\sqrt{N})$，空间复杂度为$O(1)$。

执行用时：4ms，在所有java提交中击败了77.60%的用户。

内存消耗：36.2MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;官方进一步利用数学规律进行简化。注意到$(2a + l - 1) * l = 2 * num$中$2a - 1$必然是奇数，如果$l$是奇数，则这两部分分别是偶数和奇数，反之是奇数和偶数，即$2*num$可由奇数和偶数之积的组合构成。问题转化为寻找有多少组这样的组合。进一步分析，偶数和奇数的积可以表示为$2^T*S$，可对$S$作分解，得到奇数分量和偶数分量，问题又转变为$2 * num$的因子分解问题。

```java
class Solution {
    public int consecutiveNumbersSum(int N) {
        // 得到最大奇因子S
        while ((N & 1) == 0) N >>= 1;

        // 包含自己本身
        int count = 1;
        // 奇数从3开始
        int d = 3;
        while (d * d <= N) {
            // 以d或d指数倍的奇数划分方案
            int t = 0;
            // 能被奇数d整除
            while (N % d == 0) {
                N /= d;
                t++;
            }

            // 当前奇数与之前奇数的组合数（奇数相乘依然是奇数）
            // 加一表示1、当前无法被d分解则t为0；2、能被d分解，但是分配到偶数中去
            count *= t + 1;

            // 迭代下一个奇数
            d += 2;
        }

        // 最后剩余N为奇数，同样要么将该数字与之前奇数组合为新的奇数，要么加入到偶数中去，乘2
        if (N > 1) count <<= 1;
        return count;
    }
}
```

&emsp;时间复杂度为$O(\sqrt{N})$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了98.70%的用户。

内存消耗：36.2MB，在所有java提交中击败了50.00%的用户。