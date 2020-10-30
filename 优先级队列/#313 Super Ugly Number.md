[toc]

Write a program to find the `n-th` super ugly number.

Super ugly numbers are positive numbers whose all prime factors are in the given prime list primes of size k.



**Note**:

* $1$ is a super ugly number for any given `primes`.
* The given numbers in `primes` are in ascending order.
* $0 < k \le 100$, $0 < n \le 106$, $0 < \text{primes[i]} < 1000$.
* The `n-th` super ugly number is guaranteed to fit in a 32-bit signed integer.



## 题目解读

&emsp;题目定义超级丑数为所有质因数为给定列表的中的数，其中$1$是任何给定的质因数的超级丑数。求第$n$个丑数。

```java
class Solution {
    public int nthSuperUglyNumber(int n, int[] primes) {
        
    }
}
```

## 程序设计

* 与[#264 Ugly Number II](./#264 Ugly Number II.md)同理，可以使用堆扩展，思路一致，但是会超时。借鉴动态规划可得：

```java
class Solution {
    public int nthSuperUglyNumber(int n, int[] primes) {
        // 动态规划
        int[] dp = new int[n];
        dp[0] = 1;
        // 记录每个指数相乘的操作数索引
        int[] points = new int[primes.length];
        // 遍历
        for(int i = 1; i < n; i++) {
            // 遍历查找相乘最小值
            int min = dp[points[0]] * primes[0];
            for(int j = 1; j < primes.length; j++) {
                if(min > dp[points[j]] * primes[j]) {
                    min = dp[points[j]] * primes[j];
                }
            }
            // 更新
            dp[i] = min;
            // 更新操作数索引
            for(int j = 0; j < primes.length; j++) {
                // 和本次最小值一致的，索引后移
                if(min == dp[points[j]] * primes[j]) {
                    points[j] += 1;
                }
            }
        }
        return dp[n - 1];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(kN)$，空间复杂度为$O(N)$。

执行用时：26ms，在所有java提交中击败了29.88%的用户。

内存消耗：40.2MB，在所有java提交中击败了9.65%的用户。

## 官方解题

&emsp;暂无，密切关注。