[toc]

Given a wooden stick of length n units. The stick is labelled from $0$ to $n$. For example, a stick of length $6$ is labelled as follows:

<img src="..\images\#1547_1.jpg" style="zoom:80%;" />


Given an integer array `cuts` where `cuts[i]` denotes a position you should perform a cut at.

You should perform the cuts in order, you can change the order of the cuts as you wish.

The cost of one cut is the length of the stick to be cut, the total cost is the sum of costs of all cuts. When you cut a stick, it will be split into two smaller sticks (i.e. the sum of their lengths is the length of the stick before the cut). Please refer to the first example for a better explanation.

Return the minimum total cost of the cuts.



**Constraints**:

* $2 \le n \le 10^6$
* $1 \le \text{cuts.length} \le \min(n - 1, 100)$
* $1 \le \text{cuts[i]} \le n - 1$
* All the integers in cuts array are **distinct**.



## 题目解读

&emsp;给定一定长度的棍子及其切分位置，求切分棍子的最小成本。切棍子的成本为当前要切棍子的长度。

```java
class Solution {
    public int minCost(int n, int[] cuts) {

    }
}
```

## 程序设计

* 采用动态规划数组`dp(i,j)`表示区间$i \sim j$切分的最小代价；则可得到转移方程`dp(i,j) = j - i + min(dp(i,k) + dp(k,j))`。

```java
class Solution {
    public int minCost(int n, int[] cuts) {
        int m = cuts.length;
        Arrays.sort(cuts);
        // 动态规划数组，需要包含棍子起始0和结束n，故长度为m+1
        // 此处如果使用原始长度n进行计算则会超时
        int[][] dp = new int[m + 2][m + 2];

        for (int len = 2; len < m + 2; len++) {
            for (int i = 0, j = i + len; j < m + 2; i++, j++) {
                dp[i][j] = Integer.MAX_VALUE;
                // 索引i、j所对应的真实长度
                int left = i == 0 ? 0 : cuts[i - 1];
                int right = j == m + 1 ? n : cuts[j - 1];
                // 遍历切分点
                for (int k = i + 1; k < j; k++) {
                    dp[i][j] = Math.min(dp[i][j], right - left + dp[i][k] + dp[k][j]);
                }
            }
        }
        return dp[0][m + 1];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(M^3)$，空间复杂度为$O(M^2)$。

执行用时：12ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.4MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。