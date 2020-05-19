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



执行用时 :1 ms, 在所有 Java 提交中击败了59.17%的用户

内存消耗 :36.5 MB, 在所有 Java 提交中击败了7.69%的用户

## 官方解题