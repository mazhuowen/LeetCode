[toc]

Given a **non-empty** array containing **only positive integers**, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.



Note:

* Each of the array element will not exceed `100`.
* The array size will not exceed `200`.



## 题目解读

&emsp;判断数组是否可分为两个子集，两个子集元素之和相等。

```java
class Solution {
    public boolean canPartition(int[] nums) {

    }
}
```

## 程序设计

* 由于两个子集元素之和相等，问题变为$n$个元素能否存在组合使得元素之和为总的一半，转化为背包问题。
* 动态规划`dp(i,j)`表示前$i$个数字中是否可组成容量为$j$的组合。


```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0, n = nums.length;
        for (int num : nums) sum += num;
        
        // 总的和为奇数，不能划分为两个相等的和的子集
        if (sum % 2 != 0) return false;

        // 转化为前n个数字是否可填满sum容量的包的问题
        sum /= 2;
        // 表示前i个数字可构成容量为j的包
        boolean[][] dp = new boolean[n + 1][sum + 1];
        for (int i = 0; i <= n; i++) dp[i][0] = true;

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= sum; j++) {
                // 背包剩余容量不足，则最大值等于前i-1个数字构成的最大值
                if (nums[i - 1] > j) dp[i][j] = dp[i - 1][j];
                else {
                    // 不加入第j个和加入第j个，取或
                    dp[i][j] = dp[i - 1][j] || dp[i - 1][j - nums[i - 1]];
                }
            }
        }
        return dp[n][sum];
    }
}
```

* 优化空间得：

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0, n = nums.length;
        for (int num : nums) sum += num;
        
        // 总的和为奇数，不能划分为两个相等的和的子集
        if (sum % 2 != 0) return false;

        // 转化为前n个数字是否可填满sum容量的包的问题
        sum /= 2;
        // 表示前i个数字可构成容量为j的包
        boolean[] cur = new boolean[sum + 1];
        boolean[] prev = new boolean[sum + 1];
        // 第一行，也就是背包容量为0初始化为true
        prev[0] = true;

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= sum; j++) {
                // 背包剩余容量不足，则最大值等于前i-1个数字构成的最大值
                if (nums[i - 1] > j) cur[j] = prev[j];
                else {
                    // 不加入第j个和加入第j个，取或
                    cur[j] = prev[j] || prev[j - nums[i - 1]];
                }
            }
            prev = cur;
            cur = new boolean[sum + 1];
        }
        return prev[sum];
    }
}
```

* 仔细观察，修改动态规划更新方向，优化为一个一维数组。

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0, n = nums.length;
        for (int num : nums) sum += num;
        
        // 总的和为奇数，不能划分为两个相等的和的子集
        if (sum % 2 != 0) return false;

        // 转化为前n个数字是否可填满sum容量的包的问题
        sum /= 2;
        // 表示前i个数字可构成容量为j的包
        boolean[] dp = new boolean[sum + 1];
        // 第一行，也就是背包容量为0初始化为true
        dp[0] = true;

        for (int i = 1; i <= n; i++) {
            for (int j = sum; j >= 0; j--) {
                // 背包剩余容量不足，则最大值等于前i-1个数字构成的最大值
                // if (nums[i - 1] > j) dp[j] = dp[j]，不变，故略去
                if (nums[i - 1] <= j) {
                    // 不加入第j个和加入第j个，取或
                    dp[j] = dp[j] || dp[j - nums[i - 1]];
                }
            }
        }
        return dp[sum];
    }
}
```

## 性能分析

&emsp;原始动态规划时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：39ms，在所有java提交中击败了31.33%的用户。

内存消耗：39.8MB，在所有java提交中击败了5.88%的用户。

&emsp;优化后空间复杂度为$O(N)$。

执行用时：36ms，在所有java提交中击败了34.23%的用户。

内存消耗：39.7MB，在所有java提交中击败了5.88%的用户。

&emsp;优化后空间复杂度为$O(N)$。

执行用时：23ms，在所有java提交中击败了52.98%的用户。

内存消耗：38.2MB，在所有java提交中击败了11.76%的用户。

## 官方解题

&emsp;暂无，密切关注。参考社区优化，当前$i$个可以组成$sum$时，不用再继续动态规划，直接返回即可。

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0, n = nums.length;
        for (int num : nums) sum += num;
        
        // 总的和为奇数，不能划分为两个相等的和的子集
        if (sum % 2 != 0) return false;

        // 转化为前n个数字是否可填满sum容量的包的问题
        sum /= 2;
        // 表示前i个数字可构成容量为j的包
        boolean[] dp = new boolean[sum + 1];
        // 第一行，也就是背包容量为0初始化为true
        dp[0] = true;

        for (int i = 1; i <= n; i++) {
            for (int j = sum; j >= 0; j--) {
                // 只要前i个可以组成sum值，返回
                if (dp[sum]) return true;
                // 背包剩余容量不足，则最大值等于前i-1个数字构成的最大值
                // if (nums[i - 1] > j) dp[j] = dp[j]，不变，故略去
                if (nums[i - 1] <= j) {
                    // 不加入第j个和加入第j个，取或
                    dp[j] = dp[j] || dp[j - nums[i - 1]];
                }
            }
        }
        return false;
    }
}
```

执行用时：16ms，在所有java提交中击败了71.28%的用户。

内存消耗：39.5MB，在所有java提交中击败了5.88%的用户。