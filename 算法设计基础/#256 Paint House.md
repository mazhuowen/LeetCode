[toc]

There are a row of n houses, each house can be painted with one of the three colors: red, blue or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by a $n \times 3$ cost matrix. For example, `costs[0][0]` is the cost of painting house $0$ with color red; `costs[1][2]` is the cost of painting house $1$ with color green, and so on... Find the minimum cost to paint all houses.



**Note**:
All costs are positive integers.



## 题目解读

&emsp;相邻的房子颜色不能一致，求涂色的最小代价。

```java
class Solution {
    public int minCost(int[][] costs) {

    }
}
```

## 程序设计

* 使用动态规划数组`dp(i,j)`表示$0 \sim i$房子涂为$j$的最小代价。

```java
class Solution {
    public int minCost(int[][] costs) {
        if (costs == null || costs.length == 0) return 0;

        int n = costs.length;
        int[][] dp = new int[n][3];
        dp[0] = costs[0];

        for (int i = 1; i < n; i++) {
            dp[i][0] = Math.min(dp[i - 1][1], dp[i - 1][2]) + costs[i][0];
            dp[i][1] = Math.min(dp[i - 1][0], dp[i - 1][2]) + costs[i][1];
            dp[i][2] = Math.min(dp[i - 1][0], dp[i - 1][1]) + costs[i][2];
        }

        return Math.min(dp[n - 1][0], Math.min(dp[n - 1][1], dp[n - 1][2]));
    }
}
```

* 空间优化得：

```java
class Solution {
    public int minCost(int[][] costs) {
        if (costs == null || costs.length == 0) return 0;

        int n = costs.length;
        int red = costs[0][0], blue = costs[0][1], green = costs[0][2];

        for (int i = 1; i < n; i++) {
            int tempRed = red, tempBlue = blue;
            red = Math.min(blue, green) + costs[i][0];
            blue = Math.min(tempRed, green) + costs[i][1];
            green = Math.min(tempRed, tempBlue) + costs[i][2];
        }

        return Math.min(red, Math.min(blue, green));
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了99.45%的用户。

内存消耗：39.3MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。