[toc]

Given a positive integer n, break it into the sum of **at least** two positive integers and maximize the product of those integers. Return the maximum product you can get.



**Note**: You may assume that *n* is not less than 2 and not larger than 58.



## 题目解读

&emsp;将数字切分，使得乘积最大。

```java
class Solution {
    public int integerBreak(int n) {

    }
}
```

## 程序设计

* 将一个数字拆分为两部分，则`dp(n) = max(j * dp(n - j), j * (n - j))`。

```java
class Solution {
    public int integerBreak(int n) {
        if (n < 2) return 0;

        int[] dp = new int[n + 1];
        dp[2] = 1;
        for (int i = 3; i <= n; i++) {
            for (int j = 1; j < i; j++) {
                dp[i] = Math.max(dp[i], Math.max(j * dp[i - j], j * (i - j)));
            }
        }
        return dp[n];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$，

执行用时：1ms，在所有java提交中击败了59.17%的用户。

内存消耗：36.5MB，在所有java提交中击败了7.69%的用户。

## 官方解题

&emsp;暂无，密切关注。社区从数学概率的角度出发，首先所有的数字都可拆分为`2`和`3`的组合，而最大乘积就是由`2`和`3`组成的，且`3`越多越好。假设最大乘积中有一个数字不是`2`或`3`，记为`x`，则`x`可继续分解为由`2`、`3`组成的数字，乘积更大。

```java
class Solution {
    public int integerBreak(int n) {
        if (n <= 3) {
            return n-1;
        }
        int i = n/3, j = n%3;
        // 能被3整除，则全部由3构成
        if (j == 0) {
            return (int) Math.pow(3, i);
        }
        // 余1，1可与3组合为4
        else if (j == 1) {
            return (int) Math.pow(3, i - 1) * 4;
        } 
        // 余2
        else {
            return (int) Math.pow(3, i) * 2;
        }
    }
}
```

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.2MB，在所有java提交中击败了7.69%的用户。