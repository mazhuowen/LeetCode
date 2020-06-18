[toc]

There are a row of n houses, each house can be painted with one of the k colors. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by a $n \times k$ cost matrix. For example, `costs[0][0]` is the cost of painting house $0$ with color $0$; `costs[1][2]` is the cost of painting house $1$ with color $2$, and so on... Find the minimum cost to paint all houses.



**Note**:
All costs are positive integers.



**Follow up:**
Could you solve it in $O(nk)$ runtime?



## 题目解读

&emsp;有$k$种颜色选择，求着色房子的最小代价。

```java
class Solution {
    public int minCostII(int[][] costs) {

    }
}
```

## 程序设计

* 最基本的思路是使用`dp(i,j)`表示第$i$栋房子着色$j$的最小代价，模拟整个着色过程。

```java
class Solution {
    public int minCostII(int[][] costs) {
        if (costs == null || costs.length == 0) return 0;

        int n = costs.length, k = costs[0].length;
        int[][] dp = new int[n][k];
        dp[0] = costs[0];

        for (int i = 0; i < n - 1; i++) {
            for (int j = 0; j < k; j++) {
                int min = Integer.MAX_VALUE;
                for (int l = 0; l < k; l++) {
                    if (j == l) continue;
                    min = Math.min(min, dp[i][l]);
                }
                dp[i + 1][j] = min + costs[i + 1][j];
            }
        }

        int res =Integer.MAX_VALUE;
        for (int cost : dp[n - 1]) {
            res = Math.min(res, cost);
        }
        return res;
    }
}
```



## 性能分析

&emsp;时间复杂度为$O(NK^2)$，空间复杂度为$O(NK)$。

执行用时：4ms，在所有java提交中击败了55.47%的用户。

内存消耗：40.1MB，在所有java提交中击败了100.00%的用户。



## 官方解题

&emsp;