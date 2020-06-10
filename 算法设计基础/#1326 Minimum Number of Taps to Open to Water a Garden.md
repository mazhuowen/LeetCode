[toc]


There is a one-dimensional garden on the x-axis. The garden starts at the point `0` and ends at the point `n`. (i.e The length of the garden is `n`).

There are `n + 1` taps located at points `[0, 1, ..., n]` in the garden.

Given an integer `n` and an integer array `ranges` of length `n + 1` where `ranges[i]` (0-indexed) means the `i-th` tap can water the area `[i - ranges[i], i + ranges[i]]` if it was open.

Return the minimum number of taps that should be open to water the whole garden, If the garden cannot be watered return -1.

**Constraints**:

* $1 \le n \le 10^4$
* `ranges.length == n + 1`
* $0 \le \text{ranges[i]} \le 100$


## 题目解读

&emsp;给定花园每个位置浇灌头的浇灌范围，求可以浇灌整个花园的最少浇灌头数目。注意花园是连续的，不是离散值，故`[0,0,0,0]`即使开启所有水龙头，不可能灌溉到整个花园。

```java
class Solution {
    public int minTaps(int n, int[] ranges) {

    }
}
```

## 程序设计

* 参考官方解题思路，首先将位置信息处理为区间信息，每个区间对应一个水龙头（多个水龙头存在一个区间，选择区间最大那个），然后采用动态规划计算；`dp(i)`表示可灌溉区间`[0,i]`的最少水龙头数目， 假设以`i`结束的区间为`[j,i]`，则只需遍历在`[j,i)`间的右区间`z`，$dp(i) = \min(dp(z)) + 1$。
* 初始化`dp(0)`为$0$。

```java
class Solution {
    public int minTaps(int n, int[] ranges) {
        if (ranges == null || ranges.length != n + 1) throw new IllegalArgumentException("invalid param");

        // 处理为每个水龙头覆盖到的区间形式，索引为右区间，值为左区间
        int[] interval = new int[n + 1];
        for (int i = 0; i <= n; i++) {
            interval[i] = i;
        }
        for (int i = 0; i <= n; i++) {
            int left = Math.max(0, i - ranges[i]);
            int right = Math.min(n, i + ranges[i]);
            // 右区间记录最小的左区间
            interval[right] = Math.min(interval[right], left);
        }

        // 表示0~i区间最少的水龙头数
        int[] dp = new int[n + 1];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0;
        for (int i = 1; i <= n; i++) {
            // 查询到达[j,i]内的区间
            for (int j = interval[i]; j < i; j++) {
                if (dp[j] == Integer.MAX_VALUE) continue;
                
                dp[i] = Math.min(dp[i], dp[j] + 1);
            }
        }
        return dp[n] == Integer.MAX_VALUE ? -1 : dp[n];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(N)$，其中$M$为最长区间长度。

执行用时：7ms，在所有java提交中击败了70.24%的用户。

内存消耗：40.2MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;除了上述思路，官方还提供了贪婪法。贪婪法的思路是选取当前位置可达的最大区间，这样得到的区间数最少。使用反证法，前面方案为`A`，假设存在区间数更少的方案`B`，则从起始出发，`A`可到达比`B`更远的位置， 即`A`所到达的区间包含了`B`所到达的区间，这样`A`方案得到的区间数不大于`B`，与假设矛盾。故贪婪法得到的区间数是最少的，对应的水龙头也最少。

```java
class Solution {
    public int minTaps(int n, int[] ranges) {
        if (ranges == null || ranges.length != n + 1) throw new IllegalArgumentException("invalid param");

        // 处理为每个水龙头覆盖到的区间形式，索引为左区间，值为右区间
        int[] interval = new int[n + 1];
        for (int i = 0; i <= n; i++) {
            int left = Math.max(0, i - ranges[i]);
            int right = Math.min(n, i + ranges[i]);
            // 左区间记录最大的右区间
            interval[left] = Math.max(interval[left], right);
        }
        // 可达区间，起始为0
        int pre = -1, cur = 0, count = 0;
        while (cur < n) {
            // (pre, cur]最远可达距离
            int max = 0;
            for (int i = cur; i > pre; i--) {
                max = Math.max(max, interval[i]);
            }
            // cur后的位置不可达，返回-1
            if (max <= cur) return -1;
            // 选择右区间为max的水龙头，计数加一
            count++;
            // 迭代
            pre = cur;
            cur = max;
        }
        return count;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：4ms，在所有java提交中击败了89.76%的用户。

内存消耗：40.3MB，在所有java提交中击败了100.00%的用户。