[toc]

We write the integers of `A` and `B` (in the order they are given) on two separate horizontal lines.

Now, we may draw connecting lines: a straight line connecting two numbers `A[i]` and `B[j]` such that:

* `A[i] == B[j]`;
* The line we draw does not intersect any other connecting (non-horizontal) line.

Note that a connecting lines cannot intersect even at the endpoints: each number can only belong to one connecting line.

Return the maximum number of connecting lines we can draw in this way.

 

**Note:**

* $1 \le \text{A.length} \le 500$
* $1 \le \text{B.length} \le 500$
* $1 \le \text{A[i], B[i]} \le 2000$



## 题目解读

&emsp;给定数组，返回最大的不相交连线数目。

```java
class Solution {
    public int maxUncrossedLines(int[] A, int[] B) {
        
    }
}
```

## 程序设计

* 问题的根本在于分析得出最大连线数实际上就是最长公共子序列长度。

```java
class Solution {
    public int maxUncrossedLines(int[] A, int[] B) {
        if (A == null || A.length == 0 || B == null || B.length == 0) return 0;
        // 动态规划数组表示字符串A的前i个和字符串B的前j个最长公共子序列
        int[][] dp = new int[A.length + 1][B.length + 1];
        for (int i = 0; i < A.length; i++) {
            for (int j = 0; j < B.length; j++) {
                // 字符相等
                if (A[i] == B[j]) dp[i + 1][j + 1] = dp[i][j] + 1;
                // 不等，则选择最大值
                else dp[i + 1][j + 1] = Math.max(dp[i + 1][j], dp[i][j + 1]);
            }
        }
        return dp[A.length][B.length];
    }
}
```

##　性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：6ms，在所有java提交中击败了93.74%的用户。

内存消耗：39.4MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;暂无，密切关注。