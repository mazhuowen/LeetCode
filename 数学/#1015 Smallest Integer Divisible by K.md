[toc]

Given a positive integer $K$, you need find the **smallest** positive integer $N$ such that $N$ is divisible by $K$, and $N$ only contains the digit $1$.

Return the length of $N$.  If there is no such $N$, return $-1$.



**Note:**

- $1 \le K \le 10^5$



## 题目解读

&emsp;查找最小的可被$K$整除的只包含$1$的数。

```java
class Solution {
    public int smallestRepunitDivByK(int K) {

    }
}
```

## 程序设计

* 枚举长度为$11\dots1$的数，最多有$10$个，以$K=3$为例，$11$显然不能整除，余$2$，继续遍历下一位$111 = 11 * 10 + 1$，此时余数$1 + 2 = 3$可被$K$整除，得到答案。

```java
class Solution {
    public int smallestRepunitDivByK(int K) {
        // 能被2整除或5整除，无法得到个位为1
        if (K % 2 == 0 || K % 5 == 0) return -1;
        // 初始数字为1，长度为1，余数为1 mod K
        int len = 1, remainder = 1 % K;
        while (remainder != 0) {
            // 遍历下一个数字，更新余数，长度增加
            remainder = (remainder * 10 + 1) % K;
            len++;
        }
        return remainder == 0 ? len : -1;
    }
}
```

## 性能分析

&emsp;在循环中有`remainder = (remainder * 10 + 1) % K;`，如果`remainder`存在相同，即造成死循环则算法会出错；假设存在两个余数为$\text{num}_i \mod K= \text{num}_j \mod K$，即$(\text{num}_{i - 1} * 10 + 1) \mod K = (\text{num}_{j - 1} * 10 + 1) \mod K$，可得$\text{num}_{i - 1} * 10 + 1 = \text{num}_{j - 1} * 10 + 1 + \lambda K \implies \text{num}_{i - 1} - \text{num}_{j - 1} = \frac{\lambda K}{10}$，由于两个余数必然小于$K$，$\lambda \in (-10, 10)$，为了使得等式成立只有$\frac{\lambda K}{10}$为整数，而当$K = 2$或$K = 5$时，$\lambda K$才有可能乘积是$10$的倍数，故当$K \ne 2$且$K \ne 5$时，循环内的余数必然不相等，肯定存在余数为$0$的时刻，由于余数各不同，根据抽屉原理时间复杂度为$O(K)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：35.2MB，在所有java提交中击败了97.78%的用户。

## 官方解题

&emsp;暂无，密切关注。