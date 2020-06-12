[toc]

Given an array `A` of `0`s and `1`s, consider $N_i$: the `i`-th subarray from `A[0]` to `A[i]` interpreted as a binary number (from most-significant-bit to least-significant-bit.)

Return a list of booleans `answer`, where `answer[i]` is true if and only if $N_i$ is divisible by 5.



**Note:**

* $1 \le \text{A.length} \le 30000$
* `A[i]` is `0` or `1`



## 题目解读

&emsp;判断二进制数字是否可被5整除。

```java
class Solution {
    public List<Boolean> prefixesDivBy5(int[] A) {

    }
}
```

## 程序设计

* 最基本的思路是位操作，但是题目中数组可达到$30000$位，显然数据类型超出`long`太多。实际位操作无非是乘$2$然后加$0$，或加$1$。实际只需关注十进制的数字最后一位是否是$5$或$0$。

```java
class Solution {
    public List<Boolean> prefixesDivBy5(int[] A) {
        if (A == null) throw new IllegalArgumentException("invalid param");
        List<Boolean> res = new LinkedList<>();

        int tail = 0;
        for (int bit : A) {
            tail <<= 1;
            tail |= bit;
            if (tail >= 10) tail -= 10;
            res.add(tail == 0 || tail == 5);
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：5ms，在所有java提交中击败了71.76%的用户。

内存消耗：41MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;暂无，密切关注。