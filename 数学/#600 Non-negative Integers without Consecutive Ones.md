[toc]

Given a positive integer $n$, find the number of **non-negative** integers less than or equal to $n$, whose binary representations do NOT contain **consecutive ones**.



**Note**: $1 \le n \le 10^9$



## 题目解读

&emsp;找到小于等于整数$n$且二进制表示不包含连续的$1$的数字的数目。

```java
class Solution {
    public int findIntegers(int num) {

    }
}
```

## 程序设计

* 如果以二进制形式看，对于不超过$i$位的数字所有组合，有$f(i) = f(i - 1) + f(i - 2)$个数字不存在连续的$1$，这样就可以得到所有位数的数目。

<img src="..\images\#600.png" style="zoom: 67%;" />

* 由于数字`num`不一定是$2^i-1$，在得到上述数组后还需要计算判断。以$10101$为例，可以拆分为计算$0 \sim 1111$，也就是$f(4)$，然后计算$10000 \sim 10101$，即$0 \sim 101$的数目，重复上述步骤，此处可分为计算$f(2)$与$1$。需注意在迭代计算时不能重复计算$0$，其次$f(i)$不包含$10\cdots$，故还需要加上这个值。
* 另一种情况`num`中存在连续的$1$，如$10111$，根据上述分析可拆分为$f(4)$与$0 \sim 111$，继续拆分为$f(2)$与$11$，由于$11$前面是$1$，且$f(2)$已包含所有两位数的情况，故只需计算$f(1)$，然后迭代提前终止。

```java
class Solution {
    public int findIntegers(int num) {
        // 表示i位的所有可能数目
        int[] f = new int[32];
        f[0] = 2; // 0,1
        f[1] = 3; // 0,1,2
        // 数字后填0、数字后填01
        for (int i = 2; i < 32; i++) f[i] = f[i - 1] + f[i - 2];

        int res = 0;
        // 最高位位数
        int bit = 31 - Integer.numberOfLeadingZeros(num);
        // 前一位是否是1
        boolean pre = false;
        while (bit >= 0) {
            // 当前位为1
            if ((num & (1 << bit)) == 1) {
                // (0,100...0)区间内的数目，需要减去0，避免重复计算
                if (bit > 0) res += f[bit - 1] - 1;
                // 前一数值为0，还需加上100...0本身
                if (!pre) res++;
                // 前一数值为1，不需要加100...0，也无需继续遍历
                else break;
                pre = true;
            } else pre = false;
            bit--;
        }
        // 加上0（0只有一个）
        return res + 1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;上述思路参考官方解题。