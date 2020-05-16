[toc]

Given a positive integer $n$ and you can do operations as follow:

* If $n$ is even, replace n with $\frac{n}{2}$.
* If $n$ is odd, you can replace $n$ with either $n + 1$ or $n - 1$.

What is the minimum number of replacements needed for $n$ to become $1$?



## 题目解读

&emsp;返回$n$转换为$1$的最小次数。

```java
class Solution {
    public int integerReplacement(int n) {

    }
}
```

## 程序设计

* 对于偶数直接移位；对于奇数，可以加一或减一，当奇数低位有多个连续的$1$时，加一使得低位为连续的$0$，之后可以移位快速转换为$1$；对于低位不连续为$1$，即最低位为$1$，之后为$0$，此时减一使得最低位有连续的$0$。
* 需要考虑两个特例，首先是最大值，连续的$1$，加一导致溢出，最大值可以加一，此时有$31$个$0$，即移动$32$次；其次是$3$，$3$按照上述逻辑需要加一，此时有两个$0$，需要移动$3$次，而实际上可以减一，只需移动$2$次。

```java
class Solution {
    public int integerReplacement(int n) {
        if (n <= 0) throw new IllegalArgumentException("invalid param");

        // 特例最大正整数
        if (n == Integer.MAX_VALUE) return 32;

        int count = 0;
        while (n > 1) {
            // 奇数
            if ((n & 1) == 1) {
                // 低位连续1（不包含特例3）
                if (n > 3 && (n & 3) == 3) n += 1;
                else n -= 1;
            }
            // 偶数除二
            else {
                n >>= 1;
            }
            count++;
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.6MB，在所有java提交中击败了16.67%的用户。

## 官方解题

&emsp;暂无，密切关注。