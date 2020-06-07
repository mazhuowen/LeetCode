[toc]

Given an array which consists of non-negative integers and an integer m, you can split the array into m non-empty continuous subarrays. Write an algorithm to minimize the largest sum among these m subarrays.

Note:
If n is the length of array, assume the following constraints are satisfied:

* $1 \le n \le 1000$
* $1 \le m \le \min(50, n)$



## 题目解读

&emsp;将数组切分为$m$份，求切割方案使得切分后所有子部分和的最大值最小，返回最小值。

```java
class Solution {
    public int splitArray(int[] nums, int m) {
        
    }
}
```

## 程序设计

* 参考官方思路，使用`dp(i,j)`表示前`i`个序列切分为`j`部分的最小的子数组最大和，则假设前`i`中的位置`k`，则$dp(i,j) = \min(\max(dp(k,j-1),sum(i) - sum(k))$。

```java
class Solution {
    public int splitArray(int[] nums, int m) {
        if (nums == null) throw new IllegalArgumentException("invalid param");

        int n = nums.length;
        // 动态规划数组表示前0~i分为j部分的最小的最大和
        int[][] dp = new int[n + 1][m + 1];
        for (int[] num : dp) {
            Arrays.fill(num, Integer.MAX_VALUE);
        }
        // 初始化
        dp[0][0] = 0;

        // 前缀和
        int[] preSum = new int[n + 1];
        for (int i = 0; i < nums.length; i++) {
            preSum[i + 1] = preSum[i] + nums[i];
        }

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= i && j <= m; j++) {
                // 将k~i分为最后一块，与前面的j - 1块比较，选择最大值；在所有轮数中选择最小值
                for (int k = j - 1; k < i; k++) {
                    dp[i][j] = Math.min(dp[i][j], Math.max(dp[k][j - 1], preSum[i] - preSum[k]));
                }
            }
        }

        return dp[n][m];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2M)$，空间复杂度为$O(MN)$。

执行用时：46ms，在所有java提交中击败了26.51%的用户。

内存消耗：37.3MB，在所有java提交中击败了33.33%的用户。

## 官方解题

&emsp;除了上述思路，官方还提供了二分法。假设限定子数组的和不超过$val$，则根据贪婪法遍历求和，大于$val$表示之前的子数组可以划分为一个，这样可得最少划分数$count$，二分查找可以在区间内二分尝试$val$，直到得到划分数与$m$相等的结果，就是答案。

```java
class Solution {
    public int splitArray(int[] nums, int m) {
        if (nums == null) throw new IllegalArgumentException("invalid param");

        // 最大值（划分为n个时的最小和），数组和（划分为1个时的最小和）
        int left = 0, right = 0;
        for (int num : nums) {
            right += num;
            left = Math.max(left, num);
        }

        while (left < right) {
            int mid = left + (right - left) / 2;
            // 统计当划分最大子数组和为mid时，最少的划分数
            if (count(nums, mid) <= m) right = mid;
            else left = mid + 1;
        }
        return left;
    }

    // 统计和小于等于target的子数组数目
    private int count(int[] nums, int target) {
        int count = 0, sum = 0;
        for (int num : nums) {
            sum += num;
            // 当前子数组加入num会大于target，即当前子数组划分为一个，然后重新开始
            if (sum > target) {
                count++;
                sum = num;
            }
        }
        // 结束剩余的划分为一个，需要加一
        return count + 1;
    }
}
```

&emsp;时间复杂度为$O(N\log_2C)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.9MB，在所有java提交中击败了33.33%的用户。