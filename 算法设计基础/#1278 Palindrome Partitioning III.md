[toc]

You are given a string `s` containing lowercase letters and an integer `k`. You need to :

* First, change some characters of `s` to other lowercase English letters.
* Then divide `s` into `k` non-empty disjoint substrings such that each substring is palindrome.

Return the minimal number of characters that you need to change to divide the string.

 

**Constraints**:

* $1 \le k \le \text{s.length} \le 100$.
* `s` only contains lowercase English letters.



## 题目解读

&emsp;求将给定字符串切分为$k$份，且各部分变换为回文的最少变换次数。

```java
class Solution {
    public int palindromePartition(String s, int k) {

    }
}
```

## 程序设计

* 最基本的思路就是`dp(i,j)`表示区间$0 \sim i$划分为$j + 1$份的最少次数。

```java
class Solution {
    public int palindromePartition(String s, int k) {
        if (s == null || s.length() == k) return 0;

        int n = s.length();
        int[][] dp = new int[n][k];
        for (int[] array : dp) Arrays.fill(array, Integer.MAX_VALUE);

        for (int i = 0; i < n; i++) {
            // 每个字符为一个回文串
            if (i < k) dp[i][i] = 0;
            dp[i][0] = changeChar(s.substring(0, i + 1));
            // 0~i划分为j+1个部分的最小改动数
            for (int j = 1; j < Math.min(i, k); j++) {
                // 0~l-1划分为j份，l~i划分为一份，即索引l从j开始
                for (int l = j; l <= i; l++) {
                    dp[i][j] = Math.min(dp[i][j], dp[l - 1][j - 1] + changeChar(s.substring(l, i + 1)));
                }
            }
        }
        return dp[n - 1][k - 1];
    }

    // 计算需要改动的数目
    private int changeChar(String s) {
        int n = s.length(), idx = 0;
        int res = 0;
        while (idx < n / 2) {
            if (s.charAt(idx) != s.charAt(n - idx - 1)) res++;
            idx++;
        }
        return res;
    }
}
```

* 可对字符串区间修改为回文数的修改次数采用记忆化搜索加速：

```java
class Solution {
    private int[][] record;

    public int palindromePartition(String s, int k) {
        if (s == null || s.length() == k) return 0;

        int n = s.length();
        int[][] dp = new int[n][k];
        record = new int[n][n];
        for (int[] array : dp) Arrays.fill(array, Integer.MAX_VALUE);
        for (int[] array : record) Arrays.fill(array, -1);

        for (int i = 0; i < n; i++) {
            // 每个字符为一个回文串
            if (i < k) dp[i][i] = 0;
            dp[i][0] = changeChar(s, 0, i);
            // 0~i划分为j+1个部分的最小改动数
            for (int j = 1; j < Math.min(i, k); j++) {
                // 0~l-1划分为j份，l~i划分为一份，即索引l从j开始
                for (int l = j; l <= i; l++) {
                    dp[i][j] = Math.min(dp[i][j], dp[l - 1][j - 1] + changeChar(s, l, i));
                }
            }
        }
        return dp[n - 1][k - 1];
    }

    // 计算需要改动的数目
    private int changeChar(String s, int start, int end) {
        if (start >= end) return 0;
        if (record[start][end] != -1) return record[start][end];
        return record[start][end] = changeChar(s, start + 1, end - 1) + (s.charAt(start) == s.charAt(end) ? 0 : 1);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^3\min(N,K))$，空间复杂度为$O(N^2)$。

执行用时：63ms，在所有java提交中击败了14.16%的用户。

内存消耗：39.8MB，在所有java提交中击败了5.88%的用户。

&emsp;优化后时间复杂度为$O(N^2\min(N,K))$，空间复杂度为$O(N^2)$。

执行用时：7ms，在所有java提交中击败了46.02%的用户。

内存消耗：38MB，在所有java提交中击败了5.88%的用户。

## 官方解题

&emsp;同上。