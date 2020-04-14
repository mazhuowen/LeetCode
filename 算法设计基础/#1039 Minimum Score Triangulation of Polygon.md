[toc]

Given `N`, consider a convex `N`-sided polygon with vertices labelled `A[0], A[i], ..., A[N-1]` in clockwise order.

Suppose you triangulate the polygon into `N-2` triangles.  For each triangle, the value of that triangle is the **product** of the labels of the vertices, and the total score of the triangulation is the sum of these values over all **N-2** triangles in the triangulation.

Return the smallest possible total score that you can achieve with some triangulation of the polygon.

 

**Note:**

* $3 \le \text{A.length} \le 50$
* $1 \le \text{A[i]} \le 100$



## 题目解读

&emsp;给定凸$N$边形的顶点数组，每个顶点对应数组值为权重，由这些顶点构成的三角形面积是三个顶点乘积。求所有顶点构成的三角形面积之和最小值。

```java
class Solution {
    public int minScoreTriangulation(int[] A) {

    }
}
```

## 程序设计

* 假设`dp(i,j)`表示数组索引$i \sim j$所对应顶点集构成的最小三角形面积和，$k$为序列中一点，则`dp(i,k)`和`dp(k,j)`只需要加上边$ij$、$ik$、$kj$组成的三角形面积就是由$i \sim j$构成的三角形面积和，求最小值就是遍历$k$取所有组合中的最小值。

```java
class Solution {
    public int minScoreTriangulation(int[] A) {
        if (A == null || A.length < 3) throw new IllegalArgumentException("invalid param");

        int n = A.length;
        int[][] dp = new int[n][n];
        for (int len = 2; len < n; len++) {
            for (int start = 0, end = start + len; end < n; start++, end++) {
                for (int k = start + 1; k < end; k++) {
                    int temp = dp[start][k] + A[start] * A[k] * A[end] + dp[k][end];

                    if (dp[start][end] == 0) dp[start][end] = temp;
                    else dp[start][end] = Math.min(dp[start][end], temp);
                }
            }
        }
        return dp[0][n - 1];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^3)$，空间复杂度为$O(N^2)$。

执行用时：5ms，在所有java提交中击败了72.41%的用户。

内存消耗：37.5MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;暂无，密切关注。