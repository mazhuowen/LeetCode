[toc]

You are given a list of non-negative integers, a1, a2, ..., an, and a target, S. Now you have 2 symbols `+` and `-`. For each integer, you should choose one from `+` and `-` as its new symbol.

Find out how many ways to assign symbols to make sum of integers equal to target `S`.



Note:

* The length of the given array is positive and will not exceed 20.
* The sum of elements in the given array will not exceed 1000.
* Your output answer is guaranteed to be fitted in a 32-bit integer.



## 题目解读

&emsp;给定一个非负整数数组和一个目标数。对于数组中的任意一个整数都可以从`+`或`-`中选择一个符号添加在前面。返回可以使最终数组和为目标数`S`的所有添加符号的方法数。

```java
class Solution {
    public int findTargetSumWays(int[] nums, int S) {

    }
}
```

## 程序设计

* 最基本的思路是回溯尝试每个组合。

```java
class Solution {
    int count;

    public int findTargetSumWays(int[] nums, int S) {
        if (nums == null || nums.length == 0) return 0;
        findTargetSumWays(nums, 0, S);
        return count;
    }

    private void findTargetSumWays(int[] nums, int idx, int target) {
        // 找到一组解
        if (idx >= nums.length) {
            if (target == 0) count++;
            return;
        }
        // 试探
        findTargetSumWays(nums, idx + 1, target - nums[idx]);
        findTargetSumWays(nums, idx + 1, target + nums[idx]);
    }
}
```

* 上述暴力回溯存在大量重复尝试的问题，引入额外空间保存遍历后的结果，避免重复计算。

```java
class Solution {
    int[][] record;

    public int findTargetSumWays(int[] nums, int S) {
        if (nums == null || nums.length == 0 || S > 1000 || S < -1000) return 0;

        // 记录索引i后数字组合是否可达到值j
        this.record = new int[20][2001];
        for (int i = 0; i < 20; i++) {
            Arrays.fill(record[i], -1);
        }
        return findTargetSumWays(nums, 0, S);
    }

    private int findTargetSumWays(int[] nums, int idx, int target) {
        if (idx >= nums.length) {
            if (target == 0) return 1;
            else return 0;
        }

        // 不符合要求
        if (target > 1000 || target < -1000) return 0;
		// 有结果，直接返回
        if (record[idx][1000 - target] != -1) return record[idx][1000 - target];

        // 试探
        record[idx][1000 - target] = findTargetSumWays(nums, idx + 1, target - nums[idx])
                + findTargetSumWays(nums, idx + 1, target + nums[idx]);
        
        return record[idx][1000 - target];
    }
}
```

* 有了上述思路，可使用动态数组直接计算。

```java
class Solution {

    public int findTargetSumWays(int[] nums, int S) {
        if (nums == null || nums.length == 0 || S > 1000 || S < -1000) return 0;

        // 记录直接i的序列组合值为j的数目
        int[][] dp = new int[nums.length][2001];
        // 特别注意当nums[0]为0时，1000-0还是1000，需要加一，不能赋值
        dp[0][1000 + nums[0]] = 1;
        dp[0][1000 - nums[0]] += 1;
        
        for (int i = 1; i < nums.length; i++) {
            for (int j = -1000; j <= 1000; j++) {
                if (dp[i - 1][1000 + j] == 0) continue;

                // 更新
                if (j + nums[i] <= 1000) {
                    dp[i][1000 + j + nums[i]] += dp[i - 1][1000 + j];
                }
                if (j - nums[i] >= -1000) {
                    dp[i][1000 + j - nums[i]] += dp[i - 1][1000 + j];
                }
            }
        }
        return dp[nums.length - 1][1000 + S];
    }
}

```

* 优化空间得：

```java
class Solution {

    public int findTargetSumWays(int[] nums, int S) {
        if (nums == null || nums.length == 0 || S > 1000 || S < -1000) return 0;

        int[] pre = new int[2001];
        int[] cur = new int[2001];
        pre[1000 + nums[0]] = 1;
        pre[1000 - nums[0]] += 1;
        for (int i = 1; i < nums.length; i++) {
            for (int j = -1000; j <= 1000; j++) {
                if (pre[1000 + j] == 0) continue;

                if (j + nums[i] <= 1000) {
                    cur[1000 + j + nums[i]] += pre[1000 + j];
                }
                if (j - nums[i] >= -1000) {
                    cur[1000 + j - nums[i]] += pre[1000 + j];
                }
            }
            pre = cur;
            cur = new int[2001];
        }
        return pre[1000 + S];
    }
}
```

## 性能分析

&emsp;基本回溯的时间复杂度为$O(2^N)$，空间复杂度为$O(N)$。

执行用时：567ms，在所有java提交中击败了38.10%的用户。

内存消耗：37.4MB，在所有java提交中击败了6.67%的用户。

&emsp;额外空间回溯的时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：12ms，在所有java提交中击败了65.91%的用户。

内存消耗：39.6MB，在所有java提交中击败了6.67%的用户。

&emsp;动态规划的时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：11ms，在所有java提交中击败了70.53%的用户。

内存消耗：39.9MB，在所有java提交中击败了6.67%的用户。

&emsp;空间优化的的动态规划时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：10ms，在所有java提交中击败了72.76%的用户。

内存消耗：39.8MB，在所有java提交中击败了6.67%的用户。

## 官方解题

&emsp;官方思路同上。参考社区思路，在动态规划前计算数组和，判断是否符合要求。

```java
class Solution {

    public int findTargetSumWays(int[] nums, int S) {
        if (nums == null || nums.length == 0 || S > 1000 || S < -1000) return 0;

        // 数组和和合法的S之和必然是偶数
        int sum = 0;
        for (int num : nums) sum += num;
        if ((sum + S) % 2 == 1) return 0;

        int[] pre = new int[2001];
        int[] cur = new int[2001];
        pre[1000 + nums[0]] = 1;
        pre[1000 - nums[0]] += 1;
        for (int i = 1; i < nums.length; i++) {
            for (int j = -1000; j <= 1000; j++) {
                if (pre[1000 + j] == 0) continue;

                if (j + nums[i] <= 1000) {
                    cur[1000 + j + nums[i]] += pre[1000 + j];
                }
                if (j - nums[i] >= -1000) {
                    cur[1000 + j - nums[i]] += pre[1000 + j];
                }
            }
            pre = cur;
            cur = new int[2001];
        }
        return pre[1000 + S];
    }
}
```

&emsp;空间优化的的动态规划时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：9ms，在所有java提交中击败了74.53%的用户。

内存消耗：40MB，在所有java提交中击败了6.67%的用户。