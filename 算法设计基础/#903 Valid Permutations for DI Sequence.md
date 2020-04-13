[toc]


We are given `S`, a length `n` string of characters from the set `{'D', 'I'}`. (These letters stand for "decreasing" and "increasing".)

A *valid permutation* is a permutation `P[0], P[1], ..., P[n]` of integers `{0, 1, ..., n}`, such that for all `i`:

- If `S[i] == 'D'`, then `P[i] > P[i+1]`, and;
- If `S[i] == 'I'`, then `P[i] < P[i+1]`.

How many valid permutations are there? Since the answer may be large, **return your answer modulo `10^9 + 7`**.



Note:

* $1 \le \text{S.length} \le 200$
* `S` consists only of characters from the set `{'D', 'I'}`.



## 题目解读

&emsp;给定只由`'D'`，`'I'`构成的长度为$n$的字符串 。代表“减少”和“增加”。定义有效排列是对整数$\{0,1,\dots,n\}$的一个排列$P[0],P[1],\dots,P[n]$，使得对所有的$i$：

* 如果`S[i] == 'D'`，那么`P[i] > P[i+1]`；
* 如果`S[i] == 'I'`，那么`P[i] < P[i+1]`。

求有多少个有效排列，因为答案可能很大，所以需要取模 $10^9 + 7$.

```java
class Solution {
    public int numPermsDISequence(String S) {

    }
}
```

## 程序设计

* 对于空字符串，此时只有一种情况即`0`；对于`D`，表示第二个数字小于第一个数字，也只有一种情况`1,0`；对于`DI`，有`1,0,2`和`2,0,1`两种情况。仔细观察规律，如果当前字符串结果是上一个字符串结果拼接得来，如`D`在空串的基础上增加了数字1，将数字0拼接到空串对应`0`后面，然后原串大于等于0的都加一就得到了`1,0`，而将1拼接到原串后面不符合减少的要求。
* 可以大胆猜测当前串是在原来串的基础上拼接数字，原串的大于等于当前串的数字加一，原串的有效性不变，而为了维护当前结果的有效性，只需要保证拼接的数字与前一数字满足字符串中的关系。
* 以此来验证`DI`，由于`I`表示增加，即拼接的数字要大于末尾数字，原序列`1,0`，且拼接数字不能大于位数2，则只能拼接1、2；拼接1时将原串中的1加一，得到`2,0,1`；拼接2得到`1,0,2`；再来验证`DID`，`D`表示减少，拼接的数字要小于等于末尾数字，则`2,0,1`可拼接0,1，得到`3,1,2,0`，`3,0,2,1`；对于`1,0,2`可拼接0,1,2，得到`2,1,3,0`，`2,0,3,1`，`1,0,3,2`。可见规律是正确的，从而得到动态规划：

```java
class Solution {
    public int numPermsDISequence(String S) {
        int n = S.length(), mod = 1_000_000_007;
        // 表示第i位数字为j（有i+1个数）
        int[][] dp = new int[n + 1][n + 1];
        // 初始化，字符串为空，此时只有一个数0在位置0
        dp[0][0] = 1;

        for (int i = 1; i <= n; i++) {
            // 对于i位的排列，数字不会超过i
            for (int j = 0; j <= i; j++) {
                // 当前数字需要小于前一位数字
                if (S.charAt(i - 1) == 'D') {
                    // 在原来的序列基础上拼接新的数j，同时原来序列中大于等于j的数加一
                    // 由于需要j小于前面的数，即j < k + 1，即i-1前一个数k至少是j，这样加一后就大于j，不改变原序列次序同时满足加入的j后的次序
                    // 由于k必然小于等于i-1，提前终止避免时间浪费
                    for (int k = j; k < i; k++) {
                        dp[i][j] += dp[i - 1][k];
                        dp[i][j] %= mod;
                    }
                } 
                // 需要大于前一位数字
                else {
                    // 同理大于等于j的数加一，为了维护增序，原先的结尾数k小于j，如果等于j，加一后会大于j
                    for (int k = 0; k < j; k++) {
                        dp[i][j] += dp[i - 1][k];
                        dp[i][j] %= mod;
                    }
                }
            }
        }

        int count = 0;
        for (int num : dp[n]) {
            count += num;
            count %= mod;
        }
        return count;
    }
}
```

* 仔细观察上述循环，里面的两层循环重复计算了值，替换为一个循环：

```java
class Solution {
    public int numPermsDISequence(String S) {
        int n = S.length(), mod = 1_000_000_007;
        int[][] dp = new int[n + 1][n + 1];
        dp[0][0] = 1;

        for (int i = 1; i <= n; i++) {
           if (S.charAt(i - 1) == 'D') {
               for (int j = i - 1; j >= 0; j--) {
                   dp[i][j] = dp[i - 1][j] + dp[i][j + 1];
                   dp[i][j] %= mod;
               }
           } else {
               for (int j = 1; j <= i; j++) {
                   dp[i][j] = dp[i - 1][j - 1] + dp[i][j - 1];
                   dp[i][j] %= mod;
               }
           }
        }

        int count = 0;
        for (int num : dp[n]) {
            count += num;
            count %= mod;
        }
        return count;
    }
}
```

* 实际上，空间也可优化，在整个过程中，只用到了当前层和前一层，使用两个一维数组表示。

```java
class Solution {
    public int numPermsDISequence(String S) {
        int n = S.length(), mod = 1_000_000_007;
        int[] pre = new int[n + 1], cur = new int[n + 1];
        pre[0] = 1;

        for (int i = 1; i <= n; i++) {
           if (S.charAt(i - 1) == 'D') {
               for (int j = i - 1; j >= 0; j--) {
                   cur[j] = pre[j] + cur[j + 1];
                   cur[j] %= mod;
               }
           } else {
               for (int j = 1; j <= i; j++) {
                   cur[j] = pre[j - 1] + cur[j - 1];
                   cur[j] %= mod;
               }
           }
           // 迭代 
           pre = Arrays.copyOf(cur, n + 1);
           Arrays.fill(cur, 0);
        }

        int count = 0;
        for (int num : pre) {
            count += num;
            count %= mod;
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^3)$，空间复杂度为$O(N^2)$。

执行用时：75ms，在所有java提交中击败了33.80%的用户。

内存消耗：39MB，在所有java提交中击败了20.00%的用户。

&emsp;循环优化后时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：4ms，在所有java提交中击败了85.21%的用户。

内存消耗：39.7MB，在所有java提交中击败了20.00%的用户。

&emsp;空间优化后时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：4ms，在所有java提交中击败了85.21%的用户。

内存消耗：39.2MB，在所有java提交中击败了20.00%的用户。

## 官方解题

&emsp;社区有种不同的思路时间性能更好。与上面`dp(i,j)`表示长度为`i+1`的序列，结尾为`j`，`j <= i`，然后每次拼接不同，`dp(i,j)`表示在`n+1`的任意排序序列中，前`i+1`个数字已满足要求，剩余未选择的数字里，有`j`个数字小于第`i`位的数字，剩余`n-i`个数字，j的取值为0到`n-i`，即`j <= n-i`。以`DID`为例，初始时`dp(0,0)`表示位置0的数字其后小于该数字的数为0，即位置0数字最小，为`0***`这一种选择；`dp(0,1)`表示位置0的数字其后小于该数字的数为1，选择第二小的数字即`1***`，只有一种选择；同理`dp(0,2)`、`dp(0,3)`分别选择`2***`、`3***`，都只有一种选择。

&emsp;第二层为`D`，相比`i-1`位置的值需要减少，即从`dp(i-1,k)`中选择第`j+1`小的元素，从而变成`dp(i,j)`，可见`k >= j+1`，而`i-1`层`k <= n - (i-1)`。根据上述分析，`dp(1,0)`可以从`dp(0,1)`的序列`1`剩余的元素中选择第1小的元素，组成`1,0`，从`dp(0,2)`的序列`2`剩余的元素中选择第一小的元素，组成`2,0`，`dp(0,3)`中选择`3,0`，共3中；同理`dp(1,1)`、`dp(1,2)`选择第二小、第3小的元素。

&emsp;第三层为`I`，相比`i-1`位置的值需要增加，即从`dp(i-1,k)`中选择除了`k`个元素之外的值，即选择剩余`n-i`个数中第`j+1`小的数从而转变为`dp(i,j)`，可知`k <= j`。根据上述分析，`dp(2,0)`从`dp(1,0)`序列`1,0`、`2,0`、`3,0`剩余元素中选择第1小的元素，组成`1,0,2`、`2,0,1`、`3,0,1`共3个；`dp(2,1)`从`dp(1,0)`序列剩余元素选择第二小的元素，组成`1,0,3`、`2,0,3`、`3,0,2`，从`dp(1,1)`的序列`3,1`、`2,1`剩余的序列中选择第二小的元素，组成`3,1,2`、`2,1,3`，共5个组合。全部过程如图（图中序列数字整体加一）。

<img src="../images/#903.png" style="zoom: 67%;" />

```java
class Solution {
    public int numPermsDISequence(String S) {
        if (S == null || S.isEmpty()) return 0;

        int n = S.length(), mod = 1_000_000_007;
        // 当前位置i后剩余的元素比i小的值有j
        int[][] dp = new int[n + 1][n + 1];
        // 初始0的位置为1
        Arrays.fill(dp[0], 1);

        for (int i = 1; i <= n; i++) {
            // 随着n的增加，j每层都减少一个
            for (int j = 0; j <= n - i; j++) {
                if (S.charAt(i - 1) == 'D') {
                    // 由于是减少，需从k个小于i-1位置的值中选择一个，k必须大于j，其次前一层比当前层多一列，k小于等于n-i+1
                    for (int k = j + 1; k <= n - i + 1; k++) {
                        dp[i][j] += dp[i - 1][k];
                        dp[i][j] %= mod;
                    }
                } else {
                    // 由于是增加，需要从后面位置选择大于i-1位置的值，
                    for (int k = 0; k <= j; k++) {
                        dp[i][j] += dp[i - 1][k];
                        dp[i][j] %= mod;
                    }
                }
            }
        }
		// 最后返回动态规划数组元素
        return dp[n][0];
    }
}
```

&emsp;时间复杂度为$O(N^3)$，空间复杂度为$O(N^2)$。

执行用时：76ms，在所有java提交中击败了20.42%的用户。

内存消耗：39.5MB，在所有java提交中击败了20.00%的用户。

&emsp;同理可做循环和空间优化：

```java
class Solution {
    public int numPermsDISequence(String S) {
        if (S == null || S.isEmpty()) return 0;

        int n = S.length(), mod = 1_000_000_007;
        // 当前位置i后剩余的元素比i小的值有j
        int[][] dp = new int[n + 1][n + 1];
        // 初始0的位置为1
        Arrays.fill(dp[0], 1);

        for (int i = 1; i <= n; i++) {
            if (S.charAt(i - 1) == 'D') {
                for (int j = n - i; j >= 0; j--) {
                    dp[i][j] = dp[i - 1][j + 1] + dp[i][j + 1];
                    dp[i][j] %= mod;
                }
            } else {
                dp[i][0] = dp[i - 1][0];
                for (int j = 1; j <= n - i; j++) {
                    dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
                    dp[i][j] %= mod;
                }
            }
        }

        return dp[n][0];
    }
}
```

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：4ms，在所有java提交中击败了85.71%的用户。

内存消耗：39.2MB，在所有java提交中击败了20.00%的用户。

```java
class Solution {
    public int numPermsDISequence(String S) {
        if (S == null || S.isEmpty()) return 0;

        int n = S.length(), mod = 1_000_000_007;
        // 当前位置i后剩余的元素比i小的值有j
        int[] pre = new int[n + 1], cur = new int[n + 1];
        // 初始0的位置为1
        Arrays.fill(pre, 1);

        for (int i = 1; i <= n; i++) {
            if (S.charAt(i - 1) == 'D') {
                for (int j = n - i; j >= 0; j--) {
                    cur[j] = pre[j + 1] + cur[j + 1];
                    cur[j] %= mod;
                }
            } else {
                cur[0] = pre[0];
                for (int j = 1; j <= n - i; j++) {
                    cur[j] = pre[j] + cur[j - 1];
                    cur[j] %= mod;
                }
            }
            pre = Arrays.copyOf(cur, n + 1);
            Arrays.fill(cur, 0);
        }

        return pre[0];
    }
}
```

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：4ms，在所有java提交中击败了85.71%的用户。

内存消耗：39.4 MB，在所有java提交中击败了20.00%的用户。