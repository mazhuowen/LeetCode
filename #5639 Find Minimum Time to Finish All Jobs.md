[toc]

You are given an integer array jobs, where jobs[i] is the amount of time it takes to complete the ith job.

There are k workers that you can assign jobs to. Each job should be assigned to exactly one worker. The working time of a worker is the sum of the time it takes to complete all jobs assigned to them. Your goal is to devise an optimal assignment such that the maximum working time of any worker is minimized.

Return the minimum possible maximum working time of any assignment.

 

Example 1:

Input: jobs = [3,2,3], k = 3
Output: 3
Explanation: By assigning each person one job, the maximum time is 3.
Example 2:

Input: jobs = [1,2,4,7,8], k = 2
Output: 11
Explanation: Assign the jobs the following way:
Worker 1: 1, 2, 8 (working time = 1 + 2 + 8 = 11)
Worker 2: 4, 7 (working time = 4 + 7 = 11)
The maximum working time is 11.


Constraints:

1 <= k <= jobs.length <= 12
1 <= jobs[i] <= 107



## 题目解读

&emsp;

```java
class Solution {
    public int minimumTimeRequired(int[] jobs, int k) {

    }
}
```

## 程序设计

* 

```java
class Solution {
    public int minimumTimeRequired(int[] jobs, int k) {
        int n = jobs.length;
        // 计算集合时间和
        int[] sum = new int[1 << n];
        for (int i = 0; i < (1 << n); i++) {
            for (int j = 0; j < n; j++) {
                if ((i & (1 << j)) != 0) sum[i] += jobs[j];
            }
        }
        // 动态规划，表示分配到第k个个人时，工作集合为j的最小工时
        int[] dp = Arrays.copyOf(sum, 1 << n);
        // 分配k次
        for (int i = 1; i < k; i++) {
            // 当前的工作状态
            for (int stat = (1 << n) - 1; stat > 0; stat--) {
                // 遍历子集
                for (int sub = stat; sub > 0; sub = (sub - 1) & stat) {
                    dp[stat] = Math.min(dp[stat], Math.max(dp[stat ^ sub], sum[sub]));
                }
            }
        }
        return dp[(1 << n) - 1];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O()$，空间复杂度为$O()$。



## 官方解题

&emsp;

```java

```

&emsp;时间复杂度为$O()$，空间复杂度为$O()$。
