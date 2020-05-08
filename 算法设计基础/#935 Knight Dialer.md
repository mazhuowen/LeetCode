[toc]

A chess knight can move as indicated in the chess diagram below:

 <img src="../images/#935_0.png" style="zoom:67%;" /> <img src="../images/#935.png" style="zoom:120%;" />

This time, we place our chess knight on any numbered key of a phone pad (indicated above), and the knight makes `N-1` hops.  Each hop must be from one key to another numbered key.

Each time it lands on a key (including the initial placement of the knight), it presses the number of that key, pressing `N` digits total.

How many distinct numbers can you dial in this manner?

Since the answer may be large, output the answer modulo $10^9 + 7$.



**Note:**

- $1 <= N <= 5000$



## 题目解读

&emsp;在数字输入键盘上跳`N`步，有多少个数字。

```java
class Solution {
    public int knightDialer(int N) {

    }
}
```

## 程序设计

* 动态数组`dp(i,j)`表示跳`i`步最后到达`j`的数目。

```java
class Solution {
    int mod = 1_000_000_007;

    public int knightDialer(int N) {
        if (N < 1) return 0;
        // 当前数字对应的可跳数字
        int[][] move = new int[][]{
                {4, 6}, {6, 8}, {7, 9}, {4, 8}, {0, 3, 9}, {},
                {0, 1, 7}, {2, 6}, {1, 3}, {2, 4}
        };
        // 表示第i步最后一个数字是j的数字数目
        int[][] dp = new int[N][10];
        Arrays.fill(dp[0], 1);

        for (int i = 0; i < N - 1; i++) {
            for (int j = 0; j <= 9; j++) {
                // 从i轮生成i+1轮的数据
                for (int num : move[j]) {
                    dp[i + 1][num] += dp[i][j];
                    dp[i + 1][num] %= mod;
                }
            }
        }
        
        int sum = 0;
        for (int i = 0; i <= 9; i++) {
            sum += dp[N - 1][i];
            sum %= mod;
        }
        return sum;
    }
}
```

* 优化空间：

```java
class Solution {
    int mod = 1_000_000_007;

    public int knightDialer(int N) {
        if (N < 1) return 0;
        // 当前数字对应的可跳数字
        int[][] move = new int[][]{
                {4, 6}, {6, 8}, {7, 9}, {4, 8}, {0, 3, 9}, {},
                {0, 1, 7}, {2, 6}, {1, 3}, {2, 4}
        };
        // 表示第i步最后一个数字是j的数字数目
        int[] cur = new int[10];
        int[] next = new int[10];
        Arrays.fill(cur, 1);

        for (int i = 0; i < N - 1; i++) {
            for (int j = 0; j <= 9; j++) {
                // 从i轮生成i+1轮的数据
                for (int num : move[j]) {
                    next[num] += cur[j];
                    next[num] %= mod;
                }
            }
            cur = next;
            next = new int[10];
        }
        
        int sum = 0;
        for (int i = 0; i <= 9; i++) {
            sum += cur[i];
            sum %= mod;
        }
        return sum;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：43ms，在所有java提交中击败了60.80%的用户。

内存消耗：39.6MB，在所有java提交中击败了50.00%的用户。

&emsp;优化后，时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：33ms，在所有java提交中击败了79.55%的用户。

内存消耗：39MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;同上。社区采用有限状态机的思路巧妙解决。仔细观察拨号盘，可将键盘分为`1,3,7,9`、`2,8`、`4,6`、`0`四组，`5`抛弃，分别标记为`A`、`B`、`C`、`D`；可见`A`能跳到`B`或`C`；而`B`只能跳到`A`两次；`C`跳到`A`两次，`D`一次；`D`能跳到`C`两次。有了这个发现，可以实现如下：

```java
class Solution {
    int mod = 1_000_000_007;

    public int knightDialer(int N) {
        if (N < 1) return 0;
        if (N == 1) return 10;

        // 将键盘分为4各组，A组有4个数字，B和C组有2个，D组有一个
        int A = 1, B = 1, C = 1, D = 1;
        for (int i = 1; i < N; i++) {
            int tempA = A, tempC = C;
            A = (B + C) % mod;
            B = (tempA + tempA) % mod;
            C = (B + D) % mod;
            D = (tempC + tempC) % mod;
        }
        int res = ((2 * A) % mod + (2 * A) % mod) % mod;
        res = (res + (2 * B) % mod) % mod;
        res = (res + (2 * C) % mod) % mod;
        res = (res + D) % mod;
        return res;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：6ms，在所有java提交中击败了97.16%的用户。

内存消耗：36.2MB，在所有java提交中击败了50.00%的用户。