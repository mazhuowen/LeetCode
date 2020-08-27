[toc]

Normally, the factorial of a positive integer $n$ is the product of all positive integers less than or equal to $n$.  For example, `factorial(10) = 10 * 9 * 8 * 7 * 6 * 5 * 4 * 3 * 2 * 1`.

We instead make a clumsy factorial: using the integers in decreasing order, we swap out the multiply operations for a fixed rotation of operations: multiply (*), divide (/), add (+) and subtract (-) in this order.

For example, `clumsy(10) = 10 * 9 / 8 + 7 - 6 * 5 / 4 + 3 - 2 * 1`.  However, these operations are still applied using the usual order of operations of arithmetic: we do all multiplication and division steps before any addition or subtraction steps, and multiplication and division steps are processed left to right.

Additionally, the division that we use is floor division such that `10 * 9 / 8` equals `11`.  This guarantees the result is an integer.

Implement the `clumsy` function as defined above: given an integer `N`, it returns the clumsy factorial of `N`.



**Note**:

* $1 \le N \le 10000$
* $-2^{31} \le \text{answer} \le 2^{31} - 1$  (The answer is guaranteed to fit within a 32-bit integer.)



## 题目解读

&emsp;题目定义了笨阶乘，求指定数字的笨阶乘。

```java
class Solution {
    public int clumsy(int N) {
        
    }
}
```

## 程序设计

* 模拟递归。

```java
class Solution {
    public int clumsy(int N) {
        if (N == 1) return 1;
        if (N == 2) return 2;
        if (N == 3) return 6;
        return N * (N - 1) / (N - 2) + N - 3 + helper(N - 4);
    }

    private int helper(int N) {
        if (N == 0) return 0;
        else if (N == 1) return -1;
        else if (N == 2) return -2;
        else if (N == 3) return -6;
        else return -N * (N - 1) / (N - 2) + N - 3 + helper(N - 4);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，凯南复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了84.85%的用户。

内存消耗：36.8MB，在所有java提交中击败了16.13%的用户。

## 官方解题

&emsp;暂无，密切关注。