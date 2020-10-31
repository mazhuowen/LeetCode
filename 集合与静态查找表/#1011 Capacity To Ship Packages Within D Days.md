[toc]

A conveyor belt has packages that must be shipped from one port to another within `D` days.

The `i`-th package on the conveyor belt has a weight of `weights[i]`.  Each day, we load the ship with packages on the conveyor belt (in the order given by `weights`). We may not load more weight than the maximum weight capacity of the ship.

Return the least weight capacity of the ship that will result in all the packages on the conveyor belt being shipped within `D` days.



**Constraints:**

- $1 \le D \le \text{weights.length} \le 50000$
- $1 \le \text{weights[i]} \le 500$



## 题目解读

&emsp;传送带上的包裹必须在`D`天内从一个港口运送到另一个港口。传送带上的第`i`个包裹的重量为`weights[i]`。每一天都会按给出重量的顺序往传送带上装载包裹。装载的重量不会超过船的最大运载重量。返回能在`D`天内将传送带上的所有包裹送达的船的最低运载能力。

```java
class Solution {
    public int shipWithinDays(int[] weights, int D) {

    }
}
```

## 程序设计

* 最基本的思路是使用动态规划，`dp(i,j)`表示将区间`[0,i]`划分为`j`份的最小容量，则`dp(i,j) = min(max(dp(k,j-1),sum(k,i)))`。时间复杂度为$O(N^2C)$，会超时。

```java
class Solution {
    public int shipWithinDays(int[] weights, int D) {
        if (weights == null || weights.length == 0) return 0;

        int n = weights.length;
        // 计算前缀和
        int[] preSum = new int[n + 1];
        for (int i = 1; i <= n; i++) preSum[i] = preSum[i - 1] + weights[i - 1]; 
        
        // 表示0~i的区间分为j块的最小容量
        int[][] dp = new int[n + 1][D + 1];
        for (int[] array : dp) {
            Arrays.fill(array, Integer.MAX_VALUE);
        }
        dp[0][0] = 0;

        // 当前序列
        for (int i = 1; i <= n; i++) {
            // j块
            for (int j = 1; j <= i && j <= D; j++) {
                // 之前序列
                for (int k = 0; k < i; k++) {
                    if (dp[k][j - 1] == Integer.MAX_VALUE) continue;
                    dp[i][j] = Math.min(dp[i][j], Math.max(dp[k][j - 1], preSum[i] - preSum[k]));
                }
            }
        }

        return dp[n][D];
    }
}
```

* 参考社区思路，采用二分查找，可以确定最高运载容量和最低运载容量，在这个区间查找根据当前容量划分的天数是否是要求天数，划分采用贪心法。

```java
class Solution {
    public int shipWithinDays(int[] weights, int D) {
        if (weights == null || weights.length == 0) return 0;

        int max = 0, sum = 0;
        for (int weight : weights) {
            max = Math.max(max, weight);
            sum += weight;
        }
        
        // 二分查找
        int left = max, right = sum;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (daysWithCapacity(weights, mid) <= D) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    // 计算天数
    private int daysWithCapacity(int[] weights, int limit) {
        int days = 0;
        int c = 0;
        for (int weight : weights) {
            if (c + weight > limit) {
                days++;
                c = 0;
            }
            c += weight;
        }
        if (c != 0) days++;
        return days;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2(sum - max))$，空间复杂度为$O(1)$。

执行用时：11ms，在所有java提交中击败了62.73%的用户。

内存消耗：43.2MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;暂无，密切关注。