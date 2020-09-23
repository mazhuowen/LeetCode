[toc]

You are given a $rows \times cols$ matrix `grid`. Initially, you are located at the top-left corner $(0, 0)$, and in each step, you can only **move right or down** in the matrix.

Among all possible paths starting from the top-left corner $(0, 0)$ and ending in the bottom-right corner $(rows - 1, cols - 1)$, find the path with the **maximum non-negative product**. The product of a path is the product of all integers in the grid cells visited along the path.

Return the maximum non-negative product **modulo** $10^9 + 7$. If the maximum product is **negative** return $-1$.

**Notice that the modulo is performed after getting the maximum product**.



**Constraints:**

- $1 \le \text{rows, cols} \le 15$
- $-4 \le \text{grid[i][j]} \le 4$



## 题目解读

&emsp;求左上到右下的最大路径乘积。

```java
class Solution {
    public int maxProductPath(int[][] grid) {

    }
}
```

## 程序设计

* 采用自顶向下的回溯方式。

```java
class Solution {
    private static final int MOD = 1_000_000_007;
    int n, m;
    // 记录路径乘积最大值
    long[][] dp1;
    // 记录路径乘积最小值
    long[][] dp2;
    
    public int maxProductPath(int[][] grid) {
        n = grid.length;
        m = grid[0].length;
        dp1 = new long[n][m];
        for (long[] array : dp1) Arrays.fill(array, Long.MIN_VALUE);
        dp2 = new long[n][m];
        for (long[] array : dp2) Arrays.fill(array, Long.MAX_VALUE);
        // 初始化
        dp1[n - 1][m - 1] = dp2[n - 1][m - 1] = grid[n - 1][m - 1];
        
        long res = backTracing(0, 0, grid)[0];
        if (res < 0) return -1;
        else return (int)(res % MOD);
    }
    
    private long[] backTracing(int curX, int curY, int[][] grid) {
        if (dp1[curX][curY] != Long.MIN_VALUE) return new long[]{dp1[curX][curY], dp2[curX][curY]};
        long max = Long.MIN_VALUE, min = Long.MAX_VALUE;
        // 向下
        if (curX + 1 < n) {
            long[] res = backTracing(curX + 1, curY, grid);
            max = grid[curX][curY] > 0 ? grid[curX][curY] * res[0] : grid[curX][curY] * res[1];
            min = grid[curX][curY] > 0 ? grid[curX][curY] * res[1] : grid[curX][curY] * res[0];
        }
        // 向右
        if (curY + 1 < m) {
            long[] res = backTracing(curX, curY + 1, grid);
            max = Math.max(max, grid[curX][curY] > 0 ? grid[curX][curY] * res[0] : grid[curX][curY] * res[1]);
            min = Math.min(min, grid[curX][curY] > 0 ? grid[curX][curY] * res[1] : grid[curX][curY] * res[0]);
        }
        dp1[curX][curY] = max;
        dp2[curX][curY] = min;
        return new long[]{max, min};
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(NM)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.6MB，在所有java提交中击败了35.95%的用户。

## 官方解题

&emsp;暂无，密切关注。