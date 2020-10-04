[toc]

Given an integer $n$, you must transform it into $0$ using the following operations any number of times:

* Change the rightmost (`0`th) bit in the binary representation of $n$.
* Change the `i`th bit in the binary representation of $n$ if the `(i-1)`th bit is set to $1$ and the `(i-2)`th through `0`th bits are set to $0$.

Return the minimum number of operations to transform $n$ into $0$.



**Constraints:**

- $0 \le n \le 10^9$



## 题目解读

&emsp;每次只能翻转最右端的比特位或连续$0$和一个$1$前的一位。求将给定数字变为$0$的最少步数。

```java
class Solution {
    public int minimumOneBitOperations(int n) {

    }
}
```

## 程序设计

* 首先引入格雷码，对于编码操作从原二进制低位进行亦或操作，即`G(i) = B(i) XOR B(i + 1)`，整体位运算为`G = B ^ (B >> 1)`；对于解码操作从格雷码高位开始当前值与解码后的前一位值亦或得到当前的解码值，即`B(i) = G(i) XOR B(i + 1)`，整体位运算有两种解法，见下；
* 格雷码的生成为以$0$起始，然后通过题目操作迭代生成，格雷码从$0$到$n$对应的操作数就是$n$对应的二进制数，而$n$到$0$可以颠倒生成顺序反向生成，即结果仍然是$n$对应的二进制数，即解码过程。
* 解码算法一如下：

```java
class Solution {
    public int minimumOneBitOperations(int n) {
        if (n < 0) throw new IllegalArgumentException("invalid param");
        if (n == 0) return 0;
        // 第一种解码算法
        int mask = n;
        while (mask > 0) {
            mask >>= 1;
            n ^= mask;
        }
        return n;
    }
}
```

* 第二种解码算法更高效，但只对$32$位整数有效：

```java
class Solution {
    public int minimumOneBitOperations(int n) {
        if (n < 0) throw new IllegalArgumentException("invalid param");
        if (n == 0) return 0;
        n ^= n >> 16;
        n ^= n >> 8;
        n ^= n >> 4;
        n ^= n >> 2;
        n ^= n >> 1;
        return n;
    }
}
```

## 性能分析

&emsp;第一种解码算法时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$。



&emsp;第二种解码算法时间复杂度为$O(1)$，空间复杂度为$O(1)$。



## 官方解题

&emsp;暂无，密切关注。社区有采用观察规律从而得到动态规划的思路。首先观察$2$的次方的数转换到$0$的规律，可见$4$转化到$0$经过$2$转换到$0$，而$4$转换到$2$的过程就是后两位的$0$转换到$2$的过程，再通过操作二得到$2$，即`dp(4) = 2 * dp(2) + 1`。

<img src="D:\Github\LeetCode\images\#5533.jpg" style="zoom: 50%;" />

对于非$2$的平方的数，如$4$转化为$5$、$6$，就是后两位转化为$1$、$2$的次数，从而`dp(5) = dp(4) - dp(1)`，`dp(6) = dp(4) - dp(2)`。

<img src="D:\Github\LeetCode\images\#5533_1.jpg" style="zoom:50%;" />

由于自底向上规模较大，采用自顶向下更好。

```java
class Solution {
    Map<Integer, Integer> record = new HashMap<>();

    public int minimumOneBitOperations(int n) {
        if (n < 0) throw new IllegalArgumentException("invalid param");
        if (n == 0) return 0;
        if (record.get(n) == null) {
            int square = getLessSquare(n);
            int res;
            // 平方
            if (square == n) res = 2 * minimumOneBitOperations(n >> 1) + 1;
            // 非平方
            else res = minimumOneBitOperations(square) - minimumOneBitOperations(n & (square - 1));
            record.put(n, res);
        }
        return record.get(n);
    }

    private int getLessSquare(int n) {
        int pre = n;
        while (n > 0) {
            pre = n;
            n &= n - 1;
        }
        return pre;
    }
}
```

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(\log_2N)$。