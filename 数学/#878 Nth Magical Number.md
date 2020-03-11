[toc]

A positive integer is magical if it is divisible by either `A` or `B`.

Return the N-th magical number.  Since the answer may be very large, **return it modulo** $10^9 + 7$.

**Note:**

* $1 \le N \le 10^9$
* $2 \le A \le 40000
* $2 \le B \le 40000$



## 题目解读

&emsp;给定两个基数，返回第$N$个神奇数。

```java
class Solution {
    public int nthMagicalNumber(int N, int A, int B) {

    }
}
```

## 程序设计

* 首先计算两个数的最大公倍数，普遍的算法是欧几里得算法。

```java
 // 最大公约数，欧几里得法（辗转相余）递归形式
    private int greatestCommonDivisor(int x, int y) {
        if(x == 0) return y;
        return greatestCommonDivisor(y % x, x);
    }
    // 非递归形式
    private int greatestCommonDivisor(int x, int y) {
        int temp;
        while(x != 0) {
            temp = x;
            x = y % x;
            y = temp;
        }
        return y;
    }
```

* 对于两个数`A`、`B`，其最小公倍数`L`是一个神奇数，假设`L`是第$M$个神奇数，则第$2M$、$3M$个神奇数是$2L$、$3L$，以此类推，最小公倍数的存在使得神奇数周期化。而最小公倍数整除`A`、`B`的个数之和减去重复相加的同时整除`A`、`B`的`L`本身，最后得到的就是公倍数的次序`M`。
* 有了`M`后，使用`M`对`N`周期化，相当于计算取余后的神奇数。此时可采用暴力迭代获取。

```java
class Solution {
    public int nthMagicalNumber(int N, int A, int B) {
        int mod = 1_000_000_007;
        // 最小公倍数Least Common Multiple
        int lcm = A * B / greatestCommonDivisor1(A, B);
        // 最小公倍数是第L个神奇数
        int L = lcm / A + lcm / B - 1;
        int t = N / L, h = N % L;

        // 由于公倍数的性质，第t×L个神奇数就是t*lcm，而N = t*L+h，只需找到第h个神奇数叠加即是答案
        long temp = (long)lcm * t;
        if(h != 0) {
            // 迭代生成h+1个神奇数（初始两个数，更新h-1次，最后得到的两个数就是第h和第h+1个神奇数）
            int num1 = A, num2 = B;
            for (int i = 0; i < h - 1; i++) {
                if(num1 < num2) num1 += A;
                else num2 += B;
            }
            // 第h个神奇数就是两个中较小的那个
            temp += Math.min(num1, num2);
        }
        // 取余
        return (int)(temp % mod);
    }
}
```

* 可以采用二分法，将问题化解为求解前$N$个数的问题，当前数前面存在$val/A + val/B - val/lcm$个神奇数，利用二分逼近，直到就是第$N$个神奇数。

```java
class Solution {
    public int nthMagicalNumber(int N, int A, int B) {
        int mod = 1_000_000_007;
        // 最小公倍数Least Common Multiple
        int lcm = A * B / greatestCommonDivisor(A, B);
        long low = 1, high = 400_000_000_000_000L;
        while (low < high) {
            long mid = low + (high - low) / 2;
            // high保持大于等于N
            if(mid / A + mid / B - mid / lcm >= N) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }
        return (int)(low % mod);
    }
}
```

## 性能分析

&emsp;计算余数个神奇数，不超过$M$，由于$M = L/A + L/B - 1 = B*lcm + A*lcm - 1$，时间复杂度为$O(A + B)$，空间复杂度为$O(1)$。

执行用时 :1 ms, 在所有 Java 提交中击败了80.00%的用户

内存消耗 :36.2 MB, 在所有 Java 提交中击败了5.88%的用户

&emsp;区间为$N*max(A,B)$，二分时间复杂度为$O(\log_2N*max(A,B))$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.3MB，在所有java提交中击败了5.88%的用户。

## 官方解题

&emsp;思路同上。