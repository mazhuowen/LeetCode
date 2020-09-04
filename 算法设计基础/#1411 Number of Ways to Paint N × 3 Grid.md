[toc]

You have a `grid` of size $n \times 3$ and you want to paint each cell of the grid with exactly one of the three colours: **Red**, **Yellow** or **Green** while making sure that no two adjacent cells have the same colour (i.e no two cells that share vertical or horizontal sides have the same colour).

You are given $n$ the number of rows of the grid.

Return the number of ways you can paint this `grid`. As the answer may grow large, the answer **must be** computed modulo $10^9 + 7$.



**Constraints:**

- $n == \text{grid.length}$
- $\text{grid[i].length} == 3$
- $1 \le n \le 5000$



## 题目解读

&emsp;给定$n \times 3$的格子及三种颜色，求不同的染色方案数。

```java
class Solution {
    public int numOfWays(int n) {

    }
}
```

## 程序设计

* 抽象题目，首先对于三种颜色在一行的填充，无非分`ABA`和`ABC`两种填充模式；假设上一行是`ABA`，则下一行可填充为`BAB`、`BCB`、`BAC`、`CAB`、`CAC`，其中`ABA`型的有三种，`ABC`型的有两种；假设上一行是`ABC`，则下一行可填充为`BAB`、`BCA`、`BCB`、`CAB`，其中`ABA`型有两种，`ABC`型也有两种，可得递归公式：

```
ABA = 3 * ABA + 2 * ABC
ABC = 2 * ABA + 2 * ABC
```

```java
class Solution {
    private static final int MOD = 1_000_000_007;

    public int numOfWays(int n) {
       long ABA = 6, ABC = 6;
       for (int i = 1; i < n; i++) {
           long tmp = ABA;
           ABA = (3 * ABA + 2 * ABC) % MOD;
           ABC = (2 * tmp + 2 * ABC) % MOD;
       }
       return (int)(ABA + ABC) % MOD;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：3ms，在所有java提交中击败了96.43%的用户。

内存消耗：36.5MB，在所有java提交中击败了69.09%的用户。

## 官方解题

&emsp;上述思路参考官方。