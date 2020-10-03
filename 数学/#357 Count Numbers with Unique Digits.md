[toc]

Given a **non-negative** integer $n$, count all numbers with unique digits, $x$, where $0 \le x < 10^n$.



**Constraints:**

- $0 \le n \le 8$



## 题目解读

&emsp;给定范围$0 \sim 10^n$，求各位数字都不同的数目。

```java
class Solution {
    public int countNumbersWithUniqueDigits(int n) {

    }
}
```

## 程序设计

* 已知一个$m$位数，则不重复的排列为$9 * A_9^{m - 1}$，遍历所有位数计算可得结果。

```java
class Solution {
    public int countNumbersWithUniqueDigits(int n) {
        if (n == 0) return 1;
        int res = 10;
        for (int i = 2; i <= n; i++) {
            res += 9 * permutation(9, i - 1);
        }
        return res;
    }

    private int permutation(int n, int p) {
        int res = 1;
        for (; p > 0; p--) {
            res *= n--;
        }
        return res;
    }
}
```

* 可优化排列的计算过程。

```java
class Solution {
    public int countNumbersWithUniqueDigits(int n) {
        int res = 1;
        for (int i = 1, permutation = 9; i <= n; permutation *= 10 - i, i++) {
            res += permutation;
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，可见复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：35.5MB，在所有java提交中击败了55.52%的用户。

## 官方解题

&emsp;暂无，密切关注。