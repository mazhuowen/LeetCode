[toc]

You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

Note: Given n will be a positive integer.



## 题目解读

&emsp;爬楼梯问题。

```java
class Solution {
    public int climbStairs(int n) {

    }
}
```

## 程序设计

* 使用递归，由于重复计算导致超时。

```java
class Solution {
    public int climbStairs(int n) {
        // 当前方式无法正好达到楼顶，返回0
        if (n < 0) return 0;
        // 正好到达楼顶
        if (n == 0) return 1;
        // 递归
        return climbStairs(n - 1) + climbStairs(n - 2);
    }
}
```

* 使用动态规划。

```java
class Solution {
    public int climbStairs(int n) {
       if (n <= 0) return 0;

       int[] dp = new int[Math.max(3, n + 1)];
       dp[1] = 1;
       dp[2] = 2;
       for (int i = 3; i <= n; i++) {
           dp[i] = dp[i - 1] + dp[i - 2];
       }
       return dp[n];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.7MB，在所有java提交中击败了5.28%的用户。

## 官方解题

&emsp;还从数学角度提供了两种思路。