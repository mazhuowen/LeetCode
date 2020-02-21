[toc]

For an integer $n$, we call $k \ge 2$ a **good base** of $n$, if all digits of $n$ base $k$ are 1.

Now given a string representing $n$, you should return the smallest good base of $n$ in string format.

Note:

* The range of n is $[3, 10^{18}]$.
* The string representing n is always valid and will not have leading zeros.



## 题目解读

&emsp;以字符串的形式给出$n$，以字符串的形式返回$n$的最小好进制。好进制指的是该进制的所有数位数为1。

```java
class Solution {
    public String smallestGoodBase(String n) {
        
    }
}
```

## 程序设计

* 由于$k$进制全部是1，假设有$m$位，则$n = 1 + k + \dots + k^{m - 1}$，可知$n >= k^{m - 1}$；另外$(k + 1)^{m - 1} = 1 + \binom{1}{m-1}k + \binom{2}{m-1}k^2 + \dots + \binom{m - 2}{m - 1}k^{m - 2} + k^{m - 1}$，可知$n \le (k + 1)^{m - 1}$。即问题转变为$k \le n^{\frac{1}{m - 1}} \le k + 1$。
* 由于$n \le 10^{18}$，假设$k = 2$最小，则二进制位数有$\frac{1 - k^m}{1 - k} = n$，带入$k$和$n$得$m = 60$。即$2 \le m \le 60$，由于$m = 2$时候$k = n - 1$，在迭代中考虑$3 \le m \le 60$的情况。

```java
class Solution {
    public String smallestGoodBase(String n) {
        long num = Long.valueOf(n);
        // 对应两位k进制的情况
        long res = num - 1;
        // 进制位数范围在3~60之间
        for(int m = 3; m <= 60; m++) {
            int k = (int)Math.pow(num, 1.0 / (m - 1));
            // k大于等于2
            if(k < 2) {
                continue;
            }
            // 计算k是否符合要求，计算等比数列
            long sum = 0L, cur = 1L;
            for(int i = 0; i < m; i++) {
                sum += cur;
                cur *= k;
            }
            if(sum == num) {
                res = k;
                break;
            }
        }
        return res + "";
    }
}
```

## 性能分析

执行用时：5ms，在所有java提交中击败了58.33%的用户。

内存消耗：38MB，在所有java提交中击败了9.38%的用户。

## 官方解题

&emsp;暂无，密切关注。