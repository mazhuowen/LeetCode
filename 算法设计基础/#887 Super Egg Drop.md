[toc]

You are given `K` eggs, and you have access to a building with `N` floors from `1` to `N`. 

Each egg is identical in function, and if an egg breaks, you cannot drop it again.

You know that there exists a floor F with $0 \le F \le N$ such that any egg dropped at a floor higher than F will break, and any egg dropped at or below floor `F` will not break.

Each move, you may take an egg (if you have an unbroken one) and drop it from any floor `X` (with $1 \le X \le N$). 

Your goal is to know **with certainty** what the value of `F` is.

What is the minimum number of moves that you need to know with certainty what `F` is, regardless of the initial value of `F`?



## 题目解读

&emsp;双蛋问题。

```java
class Solution {
    public int superEggDrop(int K, int N) {

    }
}
```

## 程序设计

* 双蛋问题核心是动态规划问题，即$N$层$K$个鸡蛋最少的尝试次数是选择$k\ (k \le N)$层楼扔鸡蛋，若碎了则继续在$k$层以下实验，鸡蛋数减少一个，若未摔碎则在高层即$N - k$层实验，鸡蛋数不变，而本次实验数是这两种情况最糟糕也就是实验次数最大的那个。需要穷举$1 \sim N$之间的所有楼层，选择实验次数最小的方案。时间复杂度为$O(N^2K)$，会超时。

```java
class Solution {
    public int superEggDrop(int K, int N) {
        if (N < 0 || K < 0) throw new IllegalArgumentException("invalid param");

        int[][] dp = new int[K + 1][N + 1];
        // 初始化，只有一层楼，不管多少个鸡蛋，都只需扔一次
        for (int i = 1; i < K; i++) {
            dp[i][1] = 1;
        }
        // 只有一个鸡蛋，有N层楼就扔N次
        for (int i = 1; i < N; i++) {
            dp[1][i] = i;
        }
        // 动态规划，i个鸡蛋j层楼
        for (int i = 2; i <= K; i++) {
            for (int j = 2; j <= N; j++) {
                // 在第k层摔碎
                for (int k = 1; k <= j; k++) {
                    // 在k层摔碎，则在ｋ－１层继续尝试；第k层未摔碎，即在剩余的j-k层尝试；由于在ｋ层进行了一次实验，所以加一
                    // 由于实验次数必须保证任何情况都能测出，取两种情况的较大值
                    int temp = Math.max(dp[i - 1][k - 1], dp[i][j - k]) + 1;
                    // 最小的实验次数就是所有情况中的最小值
                    if (dp[i][j] == 0 || dp[i][j] > temp) dp[i][j] = temp;
                }
            }
        }

        return dp[K][N];
    }
}
```

* 参考官方解题，由于`dp(i, j)`鸡蛋不变，随着楼层的增大，检测次数会增大；而在第三层循环中是寻找所有可能方案中最小检测数，由上面分析知`dp(i-1,k)`和`dp(i,j-k)`随着`k`的增大分别递增和减少，而我们需要找出$\min_{1 \le k \le j}(\max(dp(i - 1, k - 1), dp(i, j - k)))$，实际上当`dp(i-1,k)`和`dp(i,j-k)`差距最小时，它们的最大值最小。这样可以使用二分来确定这个边界。

```java
class Solution {
    public int superEggDrop(int K, int N) {
        if (N <= 0 || K <= 0) throw new IllegalArgumentException("invalid param");

        int[][] dp = new int[K + 1][N + 1];
        // 初始化，只有一层楼，不管多少个鸡蛋，都只需扔一次
        for (int i = 1; i <= K; i++) {
            dp[i][1] = 1;
        }
        // 只有一个鸡蛋，有N层楼就扔N次
        for (int i = 1; i <= N; i++) {
            dp[1][i] = i;
        }
        // 动态规划，i个鸡蛋j层楼
        for (int i = 2; i <= K; i++) {
            for (int j = 2; j <= N; j++) {
                // j+1为标志位，表示不存在dp[i - 1][mid - 1] >= dp[i][j - mid]，会搜索到j+1
                int left = 1, right = j + 1;
                while (left < right) {
                    int mid = left + (right - left) / 2;
                    if (dp[i - 1][mid - 1] >= dp[i][j - mid]) {
                        right = mid;
                    } else {
                        left = mid + 1;
                    }
                }
                dp[i][j] = (left == j + 1 ? dp[i][j - left] : dp[i - 1][left - 1]) + 1;
            }
        }

        return dp[K][N];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NK\log_2N)$，空间复杂度为$O(NK)$。

执行用时：470ms，在所有java提交中击败了6.10%的用户。

内存消耗：40.2MB，在所有java提交中击败了30.77%的用户。

## 官方解题

&emsp;官方还从数学的角度提出思路优化，社区时间性能较好的方法在这个基础上进一步优化空间，代码如下：

```java
class Solution {
    public int superEggDrop(int K, int N) {
        int[] dp = new int[K + 1];
        int ans = 0;    // 操作的次数
        while (dp[K] < N){
            for (int i = K; i > 0; i--) // 从后往前计算
                dp[i] = dp[i] + dp[i-1] + 1;
            ans++;
        }
        return ans;
    }
}
```

&emsp;时间复杂度为$O(NK)$，空间复杂度为$O(K)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.4MB，在所有java提交中击败了46.15%的用户。