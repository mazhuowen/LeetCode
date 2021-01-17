[toc]

You are given an integer array `jobs`, where `jobs[i]` is the amount of time it takes to complete the `ith` job.

There are $k$ workers that you can assign jobs to. Each job should be assigned to **exactly** one worker. The **working time** of a worker is the sum of the time it takes to complete all jobs assigned to them. Your goal is to devise an optimal assignment such that the **maximum working time** of any worker is **minimized**.

Return the **minimum** possible **maximum working time** of any assignment.

 

**Example 1**:

```
Input: jobs = [3,2,3], k = 3
Output: 3
Explanation: By assigning each person one job, the maximum time is 3.
```

**Example 2**:

```
Input: jobs = [1,2,4,7,8], k = 2
Output: 11
Explanation: Assign the jobs the following way:
Worker 1: 1, 2, 8 (working time = 1 + 2 + 8 = 11)
Worker 2: 4, 7 (working time = 4 + 7 = 11)
The maximum working time is 11.
```



**Constraints**:

* $1 \le k \le \text{jobs.length} \le 12$
* $1 \le \text{jobs[i]} \le 10^7$



## 题目解读

&emsp;给定任务所需的时间和可分配任务的员工数目，求分配员工的最小的最大工作时间。

```java
class Solution {
    public int minimumTimeRequired(int[] jobs, int k) {

    }
}
```

## 程序设计

* 由于工作数目最高只有$12$个，可使用二进制数来表示工作分配情况；可使用动态规划数组`dp(i)`表示分配到当前人时，工作分配状态为$i$的最小最大工作时间，则分配到第$j$个人时，$1 \le j \le k$，`dp(i) = min(dp(i), max(dp(sub), sum(i - sub)))`，其中`sub`为子状态，`i - sub`为分配当前人员的工作状态，取最大值，如果比之前的`dp(i)`大则当前工人不分配任务。

```java
class Solution {
    public int minimumTimeRequired(int[] jobs, int k) {
        int n = jobs.length;
        // 预处理，计算集合时间和
        int[] sum = new int[1 << n];
        for (int i = 0; i < (1 << n); i++) {
            for (int j = 0; j < n; j++) {
                if ((i & (1 << j)) != 0) sum[i] += jobs[j];
            }
        }
        // 动态规划，表示分配到第k个人时，工作集合为j的最小工时
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

&emsp;时间复杂度为$O(K3^N)$，空间复杂度为$O(2^N)$。

执行用时：130 ms, 在所有 Java 提交中击败了52.32%的用户。

内存消耗：36.5 MB, 在所有 Java 提交中击败了61.08%的用户。

## 官方解题

&emsp;&emsp;暂无，密切关注。社区还有进一步优化的思路，注意到如果知晓每个工人分配工作的最大时间，`dp(i)`表示工作集合$i$所需的最少人数，则可使用二分查找来判断每个工人分配的最小的最大工作时间（因为限定的工作时间越少，需要工人的数目越多，是一种单调关系，故可使用二分查找）。

```java
class Solution {
    public int minimumTimeRequired(int[] jobs, int k) {
        int n = jobs.length;
        // 计算集合时间和
        int[] sum = new int[1 << n];
        for (int i = 0; i < (1 << n); i++) {
            for (int j = 0; j < n; j++) {
                if ((i & (1 << j)) == 0) continue;
                // 优化加速
                sum[i] = sum[i - (1 << j)] + jobs[j];
                break;
            }
        }

        // 计算可分配员工的最小最大工作时间上下界
        int left = 0, right = 0;
        for (int job : jobs) {
            left = Math.max(left, job);
            right += job;
        }

        while (left < right) {
            int mid = left + (right - left) / 2;
            // 动态规划数组，表示最大分配时间为mid时，所需的最小工作人数
            int[] dp = new int[1 << n];
            Arrays.fill(dp, Integer.MAX_VALUE / 2);
            dp[0] = 0;

            for (int i = 0; i < (1 << n); i++) {
                for (int sub = i; sub > 0; sub = (sub - 1) & i) {
                    if (sum[sub] > mid) continue;
                    // 分配人数取最小值
                    dp[i] = Math.min(dp[i], dp[i - sub] + 1);
                }
            }

            // 判断所有工作分配人数是否少于K，少于则继续逼近
            if (dp[(1 << n) - 1] <= k) right = mid;
            else left = mid + 1;
        }
        
        return left;
    }
}
```

&emsp;时间复杂度为$O(3^N\sum_{i = 0}^N\text{jobs}[i])$，空间复杂度为$O(2^N)$。

执行用时：276 ms, 在所有 Java 提交中击败了26.55%的用户。

内存消耗：38.1 MB, 在所有 Java 提交中击败了24.23%的用户。