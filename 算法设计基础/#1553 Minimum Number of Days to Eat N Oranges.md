[toc]

There are $n$ oranges in the kitchen and you decided to eat some of these oranges every day as follows:

* Eat one orange.
* If the number of remaining oranges ($n$) is divisible by $2$ then you can eat  $n/2$ oranges.
* If the number of remaining oranges ($n$) is divisible by $3$ then you can eat  $2*(n/3)$ oranges.

You can only choose one of the actions per day.

Return the minimum number of days to eat $n$ oranges.



**Constraints:**

- $1 \le n \le 2*10^9$



## 题目解读

&emsp;给定$n$个橘子，每天可以选择吃一个、吃一半（偶数个的情况）、吃三分之二（可被三整除的情况），求吃完所有句子所花的最短天数。

```java
class Solution {
    public int minDays(int n) {

    }
}
```

## 程序设计

* 最基本的思路是动态规划，`dp(i)`表示吃完$i$个橘子的最短天数。该方法不符合题目内存限制。

```java
class Solution {
    public int minDays(int n) {
        int[] dp = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            dp[i] = dp[i - 1] + 1;
            if (i % 2 == 0) {
                dp[i] = Math.min(dp[i], dp[i / 2] + 1);
            }
            if (i % 3 == 0) {
                dp[i] = Math.min(dp[i], dp[i / 3] + 1);
            }
        }
        return dp[n];
    }
}
```

* 动态规划遍历计算了所有的子问题，事实上大量都不需要计算，如果当前橘子数能被二或三整除，则吃一半或吃三分之一必然是最佳选择，不能被整除的则先吃数个橘子直到能被二或三整除，经过剪枝后得到回溯如下：

```java
class Solution {
    private Map<Integer, Integer> record = new HashMap<>();

    public int minDays(int n) {
        if (record.containsKey(n)) return record.get(n);

        if (n == 0) return 0;
        if (n == 1) return 1;
        int res = Math.min(minDays(n / 2) + n % 2, minDays(n / 3) + n % 3) + 1;
        record.put(n, res);
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：4ms，在所有java提交中击败了90.03%的用户。

内存消耗：39.1MB，在所有java提交中击败了35.99%的用户。

## 官方解题

&emsp;暂无，密切关注。