[toc]

The **Fibonacci numbers**, commonly denoted `F(n)` form a sequence, called the **Fibonacci sequence**, such that each number is the sum of the two preceding ones, starting from `0` and `1`. That is,

```
F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), for N > 1.
```

**Note:**$0 \le N \le 30$.



## 题目解读

&emsp;斐波那契数列。

```java
class Solution {
    public int fib(int N) {

    }
}
```

## 程序设计

* 使用动态规划实现。

```java
class Solution {
    int size = 1;
    int[] fib = new int[31];
    {
        fib[0] = 0;
        fib[1] = 1;
    }

    public int fib(int N) {
        if (N <= size) return fib[N];

        for (int i = size + 1; i <= N; i++) {
            fib[i] = fib[i - 1] + fib[i - 2];
        }
        size = N;
        return fib[N];
    }
}
```

> 动态规划数组可以简化为两个变量。

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.4MB，在所有java提交中击败了5.26%的用户。

## 官方解题

&emsp;还从数学角度提供了两种思路。