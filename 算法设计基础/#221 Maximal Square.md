[toc]

Given a 2D binary matrix filled with `0`'s and `1`'s, find the largest square containing only `1`'s and return its area.



## 题目解读

&emsp;寻找最大的正方形区域。

```java
class Solution {
    public int maximalSquare(char[][] matrix) {

    }
}
```

## 程序设计

* `dp(i,j)`表示以`(i,j)`为右下角顶点的最大正方形边长，则可知`(i,j)`向左至少可到达`dp(i,j-1)+1`长度，向上至少可到达`dp(i-1,j)+1`，对角线可到达`dp(i-1,j-1)+1`，以`(i,j)`为顶点的最大正方形边长就是上面三个值的最小值。

```java
class Solution {
    public int maximalSquare(char[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return 0;

        int m = matrix.length, n = matrix[0].length;
        int[][] dp = new int[m][n];
        int maxLen = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == '0') continue;

                if (i == 0 || j == 0) dp[i][j] = 1;
                else {
                    dp[i][j] = Math.min(dp[i - 1][j - 1], Math.min(dp[i - 1][j], dp[i][j - 1])) + 1;
                }

                maxLen = Math.max(maxLen, dp[i][j]);
            }
        }
        return maxLen * maxLen; 
    }
}
```

* 为了方便边界计算，引入额外的行和列，继续优化空间得：

```java
class Solution {
    public int maximalSquare(char[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return 0;

        int m = matrix.length, n = matrix[0].length;
        // 引入额外的行列，初始为0
        int[] dp = new int[n + 1];
        // 记录i-1，j-1的值
        int pre = 0;
        
        int maxLen = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int temp = dp[j + 1];
                // 一维数组不能省略为0时的赋值
                if (matrix[i][j] == '0') dp[j + 1] = 0;
                else {
                    dp[j + 1] = Math.min(dp[j], Math.min(dp[j + 1], pre)) + 1;
                }
                maxLen = Math.max(maxLen, dp[j + 1]);
                pre = temp;
            }
        }
        return maxLen * maxLen; 
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：7ms，在所有java提交中击败了47.74%的用户。

内存消耗：42.6MB，在所有java提交中击败了68.75%的用户。

&emsp;优化后时间复杂度为$O(MN)$，空间复杂度为$O(N)$。

执行用时：6ms，在所有java提交中击败了87.56%的用户。

内存消耗：43.6MB，在所有java提交中击败了12.50%的用户。

## 官方解题

&emsp;同上。