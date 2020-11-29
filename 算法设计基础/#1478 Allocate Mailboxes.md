[toc]

Given the array `houses` and an integer $k$. where `houses[i]` is the location of the ith house along a street, your task is to allocate $k$ mailboxes in the street.

Return the **minimum** total distance between each house and its nearest mailbox.

The answer is guaranteed to fit in a 32-bit signed integer.



**Constraints**:

* $n == \text{houses.length}$
* $1 \le n \le 100$
* $1 \le \text{houses[i]} \le 10^4$
* $1 \le k \le n$
* Array `houses` contain unique integers.



## 题目解读

&emsp;对$n$个房子分配$k$个邮箱，求房子到邮箱的距离之和最小值。

```java
class Solution {
    public int minDistance(int[] houses, int k) {

    }
}
```

## 程序设计

* 使用动态规划数组`dp(i,j)`表示序列$0 \sim i$分配$j$个邮箱的最少距离和；则`dp(i,j)=min(dp(l,j-1)+dis(l+1,i))`，其中`dis(l+1,i)`表示区间$l+1 \sim i$共用一个邮箱的最短距离。
* 由数学知识可知，当邮箱为区间中位数位置时，绝对值之和最小。

```java
class Solution {
    public int minDistance(int[] houses, int k) {
        int n = houses.length;
        if (k >= n) return 0;

        Arrays.sort(houses);
        // 表示i~j公用一个邮箱时的最短距离
        int[][] dis = new int[n][n];
        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                int mid = (i + j) / 2;
                for (int l = i; l <= j; l++) {
                    dis[i][j] += Math.abs(houses[l] - houses[mid]);
                }
            }
        }

        // 动态规划dp(i,j)表示前0~i个房子分配j个邮箱的最少距离之和
        int[][] dp = new int[n + 1][k + 1];
        for (int[] array : dp) Arrays.fill(array, Integer.MAX_VALUE);
        dp[0][0] = 0;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= i && j <= k; j++) {
                for (int l = j - 1; l < i; l++) {
                    if (dp[l][j - 1] == Integer.MAX_VALUE) continue;

                    dp[i][j] = Math.min(dp[i][j], dp[l][j - 1] + dis[l][i - 1]);
                }
            }
        }

        return dp[n][k];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^3)$，空间复杂度为$O(N^2)$。

执行用时：10ms，在所有java提交中击败了85.71%的用户。

内存消耗：39.5MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;暂无，密切关注。