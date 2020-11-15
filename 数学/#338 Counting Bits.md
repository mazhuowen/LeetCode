[toc]

Given a non negative integer number `num`. For every numbers i in the range $0 \le i \le num$ calculate the number of 1's in their binary representation and return them as an array.



**Follow up**:

* It is very easy to come up with a solution with run time $O(n*\text{sizeof}(\text{integer}))$. But can you do it in linear time $O(n)$ possibly in a single pass?
* Space complexity should be $O(n)$.
* Can you do it like a boss? Do it without using any builtin function like **__builtin_popcount** in c++ or in any other language.



## 题目解读

&emsp;求$0 \sim N$的每个数字二进制形式$1$的个数。

```java
class Solution {
    public int[] countBits(int num) {

    }
}
```

## 程序设计

* 最基本的思路是对每一个数字统计$1$的个数，但这样时间复杂度为$O(MN)$，不是题目要求的$O(N)$；由于数字是连续的，每次都是在上一次的基础上加一， 这样只需异或两个数字就可以得到改变值：高位为$1$表示之前是$0$，加$1$后变为$1$，低位为$0$表示低位连续的$1$加$1$后变为$0$。这样新的数字新增了一个$1$，减少了低位数个$1$。
* 但该方法仍然不是$O(N)$的算法，因为判断异或低位的$0$仍然需要遍历数字位。

```java
class Solution {
    public int[] countBits(int num) {
        if (num < 0) throw new IllegalArgumentException("invalid param");

        int[] res = new int[num + 1];
        for (int i = 1; i <= num; i++) {
            int xor = (i - 1) ^ i;

            res[i] = res[i - 1] + 2 - Integer.numberOfTrailingZeros(xor + 1);
        }

        return res;
    }
}
```

* 如果从最高位的$1$的角度出发，则问题转化为高位的一个$1$和低位的$1$的数目之和；低位数字为当前数字减去最接近的$2^m$，$m$表示当前的最高位位数。

```java
class Solution {
    public int[] countBits(int num) {
        if (num < 0) throw new IllegalArgumentException("invalid param");

        int[] res = new int[num + 1];
        // 2^m中m的值，表示当前数字的最高位
        int bit = 0;
        for (int i = 1; i <= num; i++) {
            // 当前数字进位，bit更新
            if (i == (1 << (bit + 1))) bit++;
            // 当前数字为最高位为一个1加上低位的1的数目
            res[i] = res[i - (1 << bit)] + 1;
        }

        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(1)$。

执行用时：3ms，在所有java提交中击败了36.35%的用户。

内存消耗：43.5MB，在所有java提交中击败了5.88%的用户。

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了78.37%的用户。

内存消耗：43.9MB，在所有java提交中击败了5.88%的用户。

## 官方解题

&emsp;除了上述思路，官方还提供了根据最低位$1$来迭代的思路。将当前数字可看作除最低位的数字和最低位，除最低位的数字可通过位操作右移得到，而最低位为$1$则在此基础上加一，否则加$0$。

```java
class Solution {
    public int[] countBits(int num) {
        if (num < 0) throw new IllegalArgumentException("invalid param");

        int[] res = new int[num + 1];
        for (int i = 1; i <= num; i++) {
            res[i] = res[i >> 1] + (i & 1);
        }

        return res;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了99.84%的用户。

内存消耗：43.4MB，在所有java提交中击败了5.88%的用户。

&emsp;最前面的方法根据前一个数字加一后改变的位数来做统计，实际上换个角度，从不改变的位数出发，当前数字的$1$的个数为未改变的数字的$1$加上改变的数字的一个$1$。

```java
class Solution {
    public int[] countBits(int num) {
        if (num < 0) throw new IllegalArgumentException("invalid param");

        int[] res = new int[num + 1];
        for (int i = 1; i <= num; i++) {
            res[i] = res[(i - 1) & i] + 1;
        }

        return res;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了99.84%的用户。

内存消耗：43.4MB，在所有java提交中击败了5.88%的用户。