[toc]

There are several stones **arranged in a row**, and each stone has an associated value which is an integer given in the array `stoneValue`.

In each round of the game, Alice divides the row into **two non-empty rows** (i.e. left row and right row), then Bob calculates the value of each row which is the sum of the values of all the stones in this row. Bob throws away the row which has the maximum value, and Alice's score increases by the value of the remaining row. If the value of the two rows are equal, Bob lets Alice decide which row will be thrown away. The next round starts with the remaining row.

The game ends when there is only **one stone remaining**. Alice's is initially **zero**.

Return the maximum score that Alice can obtain.



**Constraints:**

- $1 \le \text{stoneValue.length} \le 500$
- $1 \le \text{stoneValue[i]} \le 10^6$



## 题目解读

&emsp;每次将石头分为两堆，丢弃总和较大的那一堆，选择较小的一堆继续操作直到只剩一个石头，求最大分数。

```java
class Solution {
    public int stoneGameV(int[] stoneValue) {
        
    }
}
```

## 程序设计

* 最基本的，动态规划数组`dp(i,j)`表示$i \sim j$之间石头的最大分数，则只需遍历切分点$k$，选择最大的`dp(i,k)+dp(k,j)`。

```java
class Solution {
    public int stoneGameV(int[] stoneValue) {
        if (stoneValue == null || stoneValue.length <= 1) return 0;
        
        int n = stoneValue.length;
        // 计算前缀和
        int[] preSum = new int[n + 1];
        for (int i = 1; i <=n; i++) preSum[i] = preSum[i - 1] +  stoneValue[i - 1];
        
        int[][] dp = new int[n][n];
        for (int len = 2; len <= n; len++) {
            for (int left = 0, right = left + len - 1; right < n; left++, right++) {
                // 切分为[left, split]与(split, right]
                for (int split = left; split < right; split++) {
                    int lSum = preSum[split + 1] - preSum[left], rSum = preSum[right + 1] - preSum[split + 1];
                    // 丢弃右边，选择左边
                    if (lSum <= rSum) {
                        dp[left][right] = Math.max(dp[left][right], lSum + dp[left][split]);
                    }
                    // 丢弃左边，选择右边
                    if (rSum <= lSum) {
                        dp[left][right] = Math.max(dp[left][right], rSum + dp[split + 1][right]);
                    }
                }
            }
        }
        return dp[0][n - 1];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^3)$，空间复杂度为$O(N^2)$。

执行用时：497ms，在所有java提交中击败了43.40%的用户。

内存消耗：40MB，在所有java提交中击败了94.52%的用户。

## 官方解题

&emsp;暂无，密切关注。上述思路虽然通过测试用例，但$O(N^3)$的时间复杂度太高，需要优化。参考社区思路，首先由于是正数，每个区间可能存在一个拐点使得左边大于等于右边（对于`1,2,5,9`的序列不存在拐点），在得到拐点`mid`后，对于`[i,j]`区间的选择就可以通过动态规划进行，而不需要遍历；动态规划数组`maxL(i,j)`表示`k`属于区间`[i,j]`最大的`dp[i][k]+sum[i][k]`，`maxR(i,j)`同理最大的`dp[k][j]+sum[k][j]`；这样提前得到`sum[i][j]`和`mid[i][j]`，得到区间的拐点`k`，如果切分点在`k`的左边，则右边值较大，选择左边，此时分数为`sum[i][split]+dp[i][split]`，可根据`maxL[i][split]`得到，避免遍历，同时更新`maxL[i][j]`；如果切分点在`k`的右边，同理。

```java
class Solution {
    public int stoneGameV(int[] stoneValue) {
        if (stoneValue == null || stoneValue.length <= 1) return 0;
        
        int n = stoneValue.length;
        // 表示i~j的区间和
        int[][] sum = new int[n][n];
        // 区间和左侧大于等于右侧的第一个位置（可能不存在，如2,3，此时值为j）
        int[][] mid = new int[n][n];
        for (int i = 0; i < n; i++) {
            sum[i][i] = stoneValue[i];
            mid[i][i] = i;
            for (int j = i + 1; j < n; j++) {
                sum[i][j] = sum[i][j - 1] + stoneValue[j];
                int tmp = mid[i][j - 1];
                while (sum[i][j] - sum[i][tmp] > sum[i][tmp]) tmp++;
                mid[i][j] = tmp;
            }
        }

        int[][] dp = new int[n][n];
        // maxL[l][r]定义为在左侧区间mid~[L,R]中最大的dp[l][mid] + sum[l][mid]
        int[][] maxL = new int[n][n];
        // 最大的dp[mid][r] + sum[mid][r]
        int[][] maxR = new int[n][n];

        for (int i = 0; i < n; i++) maxL[i][i] = maxR[i][i] = stoneValue[i];
        
        for (int len = 2; len <= n; len++) {
            for (int left = 0, right = left + len - 1; right < n; left++, right++) {
                // 左右之和平衡点
                int m = mid[left][right];
                // 不存在平衡点时rSum赋值为0（即左侧永远小于右侧）
                int lSum = sum[left][m], rSum = m + 1 <= right ? sum[m + 1][right] : 0;
                if (lSum == rSum) {
                    dp[left][right] = Math.max(dp[left][right], maxL[left][m]);
                    dp[left][right] = Math.max(dp[left][right], maxR[m + 1][right]);
                } 
                // 此时m所在区分位置，lSum > rSum，m-1位置lsum < rSum
                else {
                    // 选择左侧（left~mid-1，左侧小于右侧，丢弃右侧）
                    if (m > left) dp[left][right] = Math.max(dp[left][right], maxL[left][m - 1]);
                    // 选择右侧（mid+1~right，左侧大于右侧，丢弃左侧）
                    if (m < right) dp[left][right] = Math.max(dp[left][right], maxR[m + 1][right]);
                }

                // 更新
                maxL[left][right] = Math.max(maxL[left][right - 1], dp[left][right] + sum[left][right]);
                maxR[left][right] = Math.max(maxR[left + 1][right], dp[left][right] + sum[left][right]);
            }
        }
        return dp[0][n - 1];
    }
}
```

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：43ms，在所有java提交中击败了98.88%的用户。

内存消耗：49.2MB，在所有java提交中击败了5.03%的用户。