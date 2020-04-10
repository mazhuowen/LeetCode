[toc]

Given `n` balloons, indexed from `0` to `n-1`. Each balloon is painted with a number on it represented by array `nums`. You are asked to burst all the balloons. If the you burst balloon `i` you will get `nums[left] * nums[i] * nums[right]` coins. Here `left` and `right` are adjacent indices of `i`. After the burst, the `left` and `right` then becomes adjacent.

Find the maximum coins you can collect by bursting the balloons wisely.

Note:

* You may imagine `nums[-1] = nums[n] = 1`. They are not real therefore you can not burst them.
* $0 \le n \le 500$, $0 \le \text{nums[i]} \le 100$



## 题目解读

&emsp;有`n`个气球，编号为`0`到`n-1`，每个气球上都标有一个数字，这些数字存在数组`nums`中。现在要戳破所有的气球。每当戳破一个气球`i`时，可以获得`nums[left] * nums[i] * nums[right]`个硬币。 这里的`left`和`right`代表和`i`相邻的两个气球的序号。当戳破气球`i`后，气球`left`和气球`right`就变成了相邻的气球。求所能获得硬币的最大数量。

```java
class Solution {
    public int maxCoins(int[] nums) {

    }
}
```

## 程序设计

* 如果依次尝试所有组合，最后返回最大收益，则时间复杂度为$O(N!)$，显然对于长度为500的数，计算量太大。换个角度考虑，实际上问题可以转化为一系列子问题，即在区间`(i,j)`戳破气球获得的最大收益等于区间`(i,k)`和`(k,j)`最大收益之和加上最后戳破`k`的收益，`k`是区间内的气球，只需遍历比较使得总体收益最大的`k`。
* 由于上述思路会重复计算，可引入数组保存结果。

```java
public class Solution{
    int[][] record;
    int left, right;

    public int maxCoins(int[] nums) {
        if (nums == null || nums.length == 0) return 0;

        left = -1;
        right = nums.length;
        // 记录戳破i，j区间内（不包括i，j）的气球的最大收益，（由于总的边界为-1～n，故数组长度加2）
        record = new int[right + 2][right + 2];

        return maxCoins(nums, left, right);
    }

    private int maxCoins(int[] nums, int start, int end) {
        // 递归终止条件，左右边界内无气球，返回0
        if (start == end - 1) {
            return 0;
        }

        // 避免重复计算
        if (record[start + 1][end + 1] > 0) return record[start + 1][end + 1];

        // 当前区间戳破气球的最大收益
        int res = 0;
        for (int i = start + 1; i < end; i++) {
            // 位置-1和n处收益为1
            int leftCoin = start == left ? 1 : nums[start];
            int rightCoin = end == right ? 1 : nums[end];
            // 假设start和end之间编号i的气球最后一个戳破，则问题可分解为：在start到i区间戳破所有气球获得最大收益，
            // 加上在i到end区间戳破所有气球最大收益，及最后戳破气球i的收益，最后气球i的左右边界是start和end。
            res = Math.max(res, maxCoins(nums, start, i) + maxCoins(nums, i, end) + leftCoin * nums[i] * rightCoin);
        }

        record[start + 1][end + 1] = res;
        return res;
    }
}
```

* 有了上述思路，可以使用动态规划来优化。

```java
public class Solution{

    public int maxCoins(int[] nums) {
        if (nums == null || nums.length == 0) return 0;

        int left = -1, right = nums.length, n = nums.length;
        // 记录戳破i，j区间内（不包括i，j）的气球的最大收益，（由于总的边界为-1～n，故数组长度加2）
        int[][] dp = new int[right + 2][right + 2];

        // i，j区间内长度从1到n（期间长度为0，即i==j-1，此时可戳破气球为0，收益为0，不必初始化）
        for (int len = 1; len <= n; len++) {
            // 根据区间得到起始和结束边界
            for (int start = left, end = start + len + 1; end <= right; start++, end++) {

                // 在区间内任选位置k作为最后戳破的气球，计算收益
                for (int k = start + 1; k < end; k++) {
                    // 考虑边界情况，-1位置和n收益为1
                    int leftCoin = start == left ? 1 : nums[start];
                    int rightCoin = end == right ? 1 : nums[end];
                    // 选择最大的收益（整体索引加一）
                    dp[start + 1][end + 1] = Math.max(dp[start + 1][end + 1],
                            dp[start + 1][k + 1] + dp[k + 1][end + 1] + leftCoin * nums[k] * rightCoin);
                }
            }
        }

        // 返回整个区间最大值
        return dp[left + 1][right + 1];
    }
}
```

* 官方形式更简洁，将左右边界加入数组，其次不同于固定区间长度遍历，而是固定左边边界遍历，这使得访问二维动态规划数组更加高效。

```java
class Solution{

    public int maxCoins(int[] nums) {
        int n = nums.length + 2;
        int[] newNums = new int[n];

        // 将左右边界加入数组
        System.arraycopy(nums, 0, newNums, 1, nums.length);
        newNums[0] = newNums[n - 1] = 1;
        
        int[][] dp = new int[n][n];
        
        for (int left = n - 2; left >= 0; left--)
            for (int right = left + 2; right < n; right++) {
                for (int i = left + 1; i < right; ++i)
                    dp[left][right] = Math.max(dp[left][right],
                            newNums[left] * newNums[i] * newNums[right] + dp[left][i] + dp[i][right]);
            }

        return dp[0][n - 1];
    }
}
```

## 性能分析

&emsp;分治法$T(N) = \sum_{i = 1}^{N - 1}(T(i) + T(N - i)) = 2T(1) + \dots + 2T(N - 1)$，时间复杂度为$O(3^N)$，加了数组记录结果后，时间复杂度为$O(2^N)$，空间复杂度为$O(N^2)$。

执行用时：12ms，在所有java提交中击败了29.97%的用户。

内存消耗：37.1MB，在所有java提交中击败了5.26%的用户。

&emsp;动态规划时间复杂度为$O(N^3)$，空间复杂度为$O(N^2)$。

执行用时：10ms，在所有java提交中击败了44.77%的用户。

内存消耗：37.2MB，在所有java提交中击败了5.26%的用户。

## 官方解题

&emsp;除了自底向上的动态规划，官方还提供了自顶向下的动态规划，实际就是上面的分治法。