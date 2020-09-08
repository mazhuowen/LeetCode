[toc]

Alex and Lee play a game with piles of stones.  There are an even number of piles **arranged in a row**, and each pile has a positive integer number of stones `piles[i]`.

The objective of the game is to end with the most stones.  The total number of stones is odd, so there are no ties.

Alex and Lee take turns, with Alex starting first.  Each turn, a player takes the entire pile of stones from either the beginning or the end of the row.  This continues until there are no more piles left, at which point the person with the most stones wins.

Assuming Alex and Lee play optimally, return `True` if and only if Alex wins the game.



**Constraints**:

* $2 \le \text{piles.length} \le 500$
* `piles.length` is even.
* $1 \le \text{piles[i]} \le 500$
* `sum(piles)` is odd.



## 题目解读

&emsp;与[#486 Predict the Winner](./#486 Predict the Winner.md)不同，这次两个选手可以从两端选择，而不是一个选择一端，另一个选择另一端。

```java
class Solution {
    public boolean stoneGame(int[] piles) {

    }
}
```

## 程序设计

* 虽然与[#486 Predict the Winner](./#486 Predict the Winner.md)不同，但实质原理相同，即动态规划数组`dp(i,j)`表示区间$i \sim j$的最大分数差。

```java
class Solution {
    public boolean stoneGame(int[] piles) {
        int n = piles.length;
        int[][] dp = new int[n][n];
        // 初始化
        for (int i = 0; i < n; i++) dp[i][i] = piles[i];

        for (int len = 2; len <= n; len++) {
            for (int left = 0, right = left + len - 1; right < n; left++, right++) {
                dp[left][right] = Math.max(piles[left] - dp[left + 1][right], piles[right] - dp[left][right - 1]);
            }
        }
        return dp[0][n - 1] > 0;
    }
}
```

* 由状态转移方程可看出`dp(left,right)`只和`dp(left+1,right)`、`dp(left,right-1)`有关，即只和`left`、`left+1`行有关，可优化为一维数组：

```java
class Solution {
    public boolean stoneGame(int[] piles) {
        int n = piles.length;
        // dp(j)表示i到j区间的最大分差
        int[] dp = new int[n];
        for (int i = 0; i < n; i++) dp[i] = piles[i];
        // 从后往前迭代
        for (int i = n - 2; i >= 0; i--) {
            for (int j = i + 1; j < n; j++) {
                dp[j] = Math.max(dp[j], dp[j - 1]);
            }
        }
        return dp[n - 1] > 0;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：7ms，在所有java提交中击败了49.49%的用户。

内存消耗：40MB，在所有java提交中击败了53.88%的用户。

&emsp;优化空间后时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：4ms，在所有java提交中击败了59.73%的用户。

内存消耗：37.2MB，在所有java提交中击败了87.58%的用户。

## 官方解题

&emsp;官方思路优化上述思路。仔细分析可知本题是[#486 Predict the Winner](./#486 Predict the Winner.md)的特例，因为增加了两个限制，即数组是偶数长，石子是奇数个，即必有胜负。官方从数学角度提出新解法。

<img src="..\images\#877.png" style="zoom: 50%;" />

假设有$n$堆石子，$n$是偶数，则每堆石子的下标从$0$到$n-1$。根据下标将$n$堆石子分成两组，每组有$\frac{n}{2}$堆石子，下标为偶数的石子堆属于第一组，下标为奇数的石子堆属于第二组。作为先手的Alex可以自由选择取走第一组的一堆石子或者第二组的一堆石子。如果Alex取走第一组的一堆石子，则剩下的部分在行的开始处和结束处的石子堆都属于第二组，因此Lee只能取走第二组的一堆石子。如果Alex取走第二组的一堆石子，则剩下的部分在行的开始处和结束处的石子堆都属于第一组，因此Lee只能取走第一组的一堆石子。无论Lee 取走的是开始处还是结束处的一堆石子，剩下的部分在行的开始处和结束处的石子堆一定是属于不同组的，因此轮到Alex取走石子时，Alex又可以在两组石子之间进行自由选择。根据上述分析可知，作为先手的Alex可以在第一次取走石子时就决定取走哪一组的石子，并全程取走同一组的石子。Alex只要选择取走数量更多的一组石子即可。因此Alex总是可以赢得比赛。

```java
class Solution {
    public boolean stoneGame(int[] piles) {
        return true;
    }
}
```

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.2MB，在所有java提交中击败了89.60%的用户。