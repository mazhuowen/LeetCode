[toc]

In a given array `nums` of positive integers, find three non-overlapping subarrays with maximum sum.

Each subarray will be of size $k$, and we want to maximize the sum of all $3*k$ entries.

Return the result as a list of indices representing the starting position of each interval (0-indexed). If there are multiple answers, return the lexicographically smallest one.



**Example**:

```
Input: [1,2,1,2,6,7,5,1], 2
Output: [0, 3, 5]
Explanation: Subarrays [1, 2], [2, 6], [7, 5] correspond to the starting indices [0, 3, 5].
We could have also taken [2, 1], but an answer of [1, 3, 5] would be lexicographically larger.
```



**Note**:

* `nums.length` will be between $1$ and $20000$.
* `nums[i]` will be between $1$ and $65535$.
* $k$ will be between $1$ and floor(nums.length / 3).



## 题目解读

&emsp;给定数组，求数组中三个长度为$k$的子数组和最大，给出起始索引。

```java
class Solution {
    public int[] maxSumOfThreeSubarrays(int[] nums, int k) {
        
    }
}
```

## 程序设计

* 在题目规定有三个子数组的情况下，可遍历得到每个索引左侧及右侧的最大单个子数组值，然后与当前段相加比较更新最大值。

```java
class Solution {
    public int[] maxSumOfThreeSubarrays(int[] nums, int k) {
        int n = nums.length;
        int[] preSum = new int[n + 1];
        for (int i = 0; i < n; i++) preSum[i + 1] = preSum[i] + nums[i];

        // 当前索引前或后的最大子数组和起始坐标
        int[] leftMax = new int[n], rightMax = new int[n];
        for (int i = k; i <= n - 2 * k; i++) {
            // 其前为i-k~i-1
            int curLeft = sum(i - k, k, preSum);
            if (i > k && curLeft <= sum(leftMax[i - 1], k, preSum)) leftMax[i] = leftMax[i - 1];
            else leftMax[i] = i - k;
        }

        for (int i = n - 2 * k; i >= k; i--) {
            // 其后为i+k~i+2*k-1
            int curRight = sum(i + k, k, preSum);
            if (i < n - 2 * k && curRight < sum(rightMax[i + 1], k, preSum)) rightMax[i] = rightMax[i + 1];
            else rightMax[i] = i + k;
        }

        int[] res = new int[3];
        int max = 0;
        for (int i = k; i <= n - 2 * k; i++) {
            // 计算中间段是i~i+k-1的最大数组和
            int sum = sum(leftMax[i], i, rightMax[i], k, preSum);
            // 更新
            if (sum > max) {
                max = sum;
                res[0] = leftMax[i];
                res[1] = i;
                res[2] = rightMax[i];
            }
        }
        return res;
    }

    private int sum(int i, int len, int[] nums) {
        return nums[i + len] - nums[i];
    }

    private int sum(int i, int j, int k, int len, int[] preSum) {
        return preSum[i + len] - preSum[i] + preSum[j + len] - preSum[j] + preSum[k + len] - preSum[k];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：4 ms, 在所有 Java 提交中击败了69.39%的用户。

内存消耗：40.6 MB, 在所有 Java 提交中击败了65.98%的用户。

## 官方解题

&emsp;官方思路类似。社区有采用动态规划的思路，可通用的解决该类问题。如果题目不再设定子数组数目为$3$，可使用`dp(i,j)`表示$0 \sim i$中有$j$段子数组的最大数组和，同时使用$path(i,j)$记录其结尾索引，这样`dp(i,j)=max(dp(i-1,j), dp(i-k,j-1) + sum(i-k+1,i))`。

```java
class Solution {
    public int[] maxSumOfThreeSubarrays(int[] nums, int k) {
        int n = nums.length;
        int[] preSum = new int[n + 1];
        for (int i = 0; i < n; i++) preSum[i + 1] = preSum[i] + nums[i];

        int[][] dp = new int[n][4], path = new int[n][4];
        // 初始化
        dp[k - 1][1] = sum(0, k, preSum);
        path[k - 1][1] = k - 1;
        
        for (int i = k; i < n; i++) {
            for (int j = 1; j <= 3; j++) {
                dp[i][j] = dp[i - 1][j];
                path[i][j] = path[i - 1][j];
                // i-k+1~i段结尾的数组和大于之前数组和，更新
                if (sum(i - k + 1, k, preSum) + dp[i - k][j - 1] > dp[i][j]) {
                    dp[i][j] = sum(i - k + 1, k, preSum) + dp[i - k][j - 1];
                    path[i][j] = i;
                }
            }
        }

        // 拼接路径
        int[] res = new int[3];
        int limit = n - 1;
        for (int i = 3; i >= 1; i--) {
            // 第i段子数组的结尾索引
            int end = path[limit][i];
            // 起始索引
            res[i - 1] = end - k + 1;
            limit = end - k;

        }
        return res;
    }

    private int sum(int i, int len, int[] nums) {
        return nums[i + len] - nums[i];
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：11 ms, 在所有 Java 提交中击败了38.78%的用户

内存消耗：41.6 MB, 在所有 Java 提交中击败了19.59%的用户。