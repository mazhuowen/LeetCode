[toc]

There is a pizza with $3n$ slices of varying size, you and your friends will take slices of pizza as follows:

* You will pick **any** pizza slice.
* Your friend Alice will pick next slice in anti clockwise direction of your pick. 
* Your friend Bob will pick next slice in clockwise direction of your pick.
* Repeat until there are no more slices of pizzas.

Sizes of Pizza slices is represented by circular array `slices` in clockwise direction.

Return the maximum possible sum of slice sizes which you can have.



**Constraints**:

* $1 \le \text{slices.length} \le 500$
* $\text{slices.length}\ \%\ 3 == 0$
* $1 \le \text{slices[i]} \le 1000$



## 题目解读

&emsp;对于一块披萨，可以选择任意一块，`Alice`选择逆时针方向第一块，`Bob`选择顺时针方向第一块；重复这个过程，直到拿完。求可以拿到的最大数值。

```java
class Solution {
    public int maxSizeSlices(int[] slices) {

    }
}
```

## 程序设计

* 分析题目，发现选择披萨的最大值起始就是求环形数组中$n$个不相邻的数字之和最大，可以发现是[#213 House Robber II](./#213 House Robber II.md)的二维变形；
* 使用`dp(i,j)`表示序列$0 \sim i$选中$j$个数的最大值，则环形数组只需要去除头、尾动态规划两次即可。

```java
class Solution {
    public int maxSizeSlices(int[] slices) {
        if (slices == null || slices.length < 1) throw new IllegalArgumentException("invalid param");

        int n = slices.length;
        // dp(i,j)表示截至i,选择了j个数
        int[][] dp = new int[n][n / 3 + 1];
        int max = 0;
        // 去除尾
        dp[1][1] = slices[0];
        for (int i = 1; i < n - 1; i++) {
            for (int j = n / 3; j >= 1; j--) {
                dp[i + 1][j] = Math.max(dp[i][j], dp[i - 1][j - 1] + slices[i]);
            }
        }
        max = dp[n - 1][n / 3];
        // 去除头
        dp = new int[n][n / 3 + 1];
        dp[1][1] = slices[1];
        for (int i = 2; i < n; i++) {
            for (int j = n / 3; j >= 1; j--) {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i - 2][j - 1] + slices[i]);
            }
        }
        max = Math.max(max, dp[n - 1][n / 3]);
        return max;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：7ms，在所有java提交中击败了92.50%的用户。

内存消耗：40MB，在所有java提交中击败了20.00%的用户。

## 官方解题

&emsp;官方除了动态规划思路还提供了时间复杂度为$O(N\log_2N)$的贪婪法，属于竞赛解法。