[toc]

Given two arrays `nums1` and `nums2`.

Return the maximum dot product between **non-empty** subsequences of nums1 and nums2 with the same length.

A subsequence of a array is a new array which is formed from the original array by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, `[2,3,5]` is a subsequence of `[1,2,3,4,5]` while `[1,5,3]` is not).



**Constraints**:

* $1 \le \text{nums1.length, nums2.length} \le 500$
* $-1000 \le \text{nums1[i], nums2[i]} \le 1000$



## 题目解读

&emsp;求两个数组子序列最大点积和。

```java
class Solution {
    public int maxDotProduct(int[] nums1, int[] nums2) {

    }
}
```

## 程序设计

* 定义动态规划数组`dp(i,j)`表示数组$0 \sim i - 1$与$0 \sim j - 1$的最大子序列点积和；则很容易想到`dp(i,j)=max(dp[i - 1][j - 1] + nums[i] * nums[j],dp[i - 1][j],dp[i][j - 1])`，此处不能忽略非常重要的情况，及点积和从当前位置重新开始，即`dp[i][j]=nums[i] * nums[j]`。
* 综合上述四种情况，为了省去判断，动态规划数组长度比数组长一，为了兼容上述方程，初始化值为`-1000001`（不去最小值，避免计算数值溢出）。

```java
class Solution {
    public int maxDotProduct(int[] nums1, int[] nums2) {
        if (nums1 == null || nums1.length == 0 || nums2 == null || nums2.length == 0) return 0;

        int m = nums1.length, n = nums2.length;
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 0; i <= n; i++) dp[0][i] = -1_000_001;
        for (int i = 0; i <= m; i++) dp[i][0] = -1_000_001;

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                // 只选择当前两个点的乘积
                dp[i][j] = nums1[i - 1] * nums2[j - 1];
                // 在之前的基础上选择当前两个点
                dp[i][j] = Math.max(dp[i][j], dp[i - 1][j - 1] + nums1[i - 1] * nums2[j - 1]);
                // 选择其中一个点
                dp[i][j] = Math.max(dp[i][j], Math.max(dp[i - 1][j], dp[i][j - 1]));
            }
        }

        return dp[m][n];
    }
}
```

* 由于每次状态只与前一个有关，可以优化空间：

```java
class Solution {
    public int maxDotProduct(int[] nums1, int[] nums2) {
        if (nums1 == null || nums1.length == 0 || nums2 == null || nums2.length == 0) return 0;

        int m = nums1.length, n = nums2.length;
        int[] dp = new int[n + 1];
        Arrays.fill(dp, -1_0000_001);

        for (int i = 1; i <= m; i++) {
            // 记录上一次动态规划数组j的左边位置值
            int pre = dp[0];
            for (int j = 1; j <= n; j++) {
                // 记录上一次动态规划数组j位置值
                int cur = dp[j];
                // 只选择当前两个点的乘积
                dp[j] = nums1[i - 1] * nums2[j - 1];
                // 在之前的基础上选择当前两个点
                dp[j] = Math.max(dp[j], pre + nums1[i - 1] * nums2[j - 1]);
                // 选择其中一个点
                dp[j] = Math.max(dp[j], Math.max(cur, dp[j - 1]));
                pre = cur;
            }
        }

        return dp[n];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：12ms，在所有java提交中击败了66.12%的用户。

内存消耗：39.1MB，在所有java提交中击败了100.00%的用户。

&emsp;优化后时间复杂度为$O(MN)$，空间复杂度为$O(N)$。

执行用时：10ms，在所有java提交中击败了84.99%的用户。

内存消耗：37.8MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。