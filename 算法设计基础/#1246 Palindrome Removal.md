[toc]

Given an integer array `arr`, in one move you can select a **palindromic** subarray `arr[i], arr[i+1], ..., arr[j]` where $i \le j$, and remove that subarray from the given array. Note that after removing a subarray, the elements on the left and on the right of that subarray move to fill the gap left by the removal.

Return the minimum number of moves needed to remove all numbers from the array.



**Constraints:**

- $1 \le \text{arr.length} \le 100$
- $1 \le \text{arr[i]} \le 20$



## 题目解读

&emsp;给定数组，给出移除的最少次数。题目要求回文字符串可一次移除。

```java
class Solution {
    public int minimumMoves(int[] arr) {
        
    }
}
```

## 程序设计

* 观察得，假设序列$i \sim j$的移除次数为$x$，考虑序列$i - 1 \sim j + 1$，如果$i - 1$和$j + 1$位置的字符相等，则移除次数也为$x$。因为$i \sim j$序列要么删除最后一次后只剩一个元素，要么剩余回文字符串，不管哪种情况，都会和$i - 1$及$j + 1$构成新的回文字符串，只需删除一次，这样这两个序列的删除次数一致。
* 如果$i - 1$和$j + 1$位置的字符不相等，则最小删除次数是子区间$i - 1 \sim k$和$k + 1 \sim j + 1$删除次数之和的最小次数。

```java
class Solution {
    public int minimumMoves(int[] arr) {
        if (arr == null || arr.length == 0) return 0;

        int n = arr.length;
        // i~j表示这段序列的最小移除数目
        int[][] dp = new int[n][n];
        // 初始化，单个字符移除为1，两个字符需判断是否相等
        for (int i = 0; i < n; i++) {
            dp[i][i] = 1;
            if (i < n - 1) {
                dp[i][i + 1] = arr[i] == arr[i + 1] ? 1 : 2;
            }
        }

        for (int len = 3; len <= n; len++) {
            for (int i = 0, j = i + len - 1; j < n; i++, j++) {
                // 字符相等，则删除次数和子区间一致
                if (arr[i] == arr[j]) dp[i][j] = dp[i + 1][j - 1];
                else dp[i][j] = Integer.MAX_VALUE;

                // 遍历查找所有子区间组合，取最小值
                for (int k = i; k < j; k++) {
                    dp[i][j] = Math.min(dp[i][j], dp[i][k] + dp[k + 1][j]);
                }
            }
        }
        return dp[0][n - 1];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^3)$，空间复杂度为$O(N^2)$。

执行用时：50ms，在所有java提交中击败了29.35%的用户。

内存消耗：39.5MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区思路一致。继续优化上述代码，将第二个循环放到外面，,将初始化放到一个循环中，这样会提高时间性能。

```java
class Solution {

    public int minimumMoves(int[] arr) {
        if (arr == null || arr.length == 0) return 0;

        int n = arr.length;
        // i~j表示这段序列的最小移除数目
        int[][] dp = new int[n][n];

        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                if (i == j) dp[i][j] = 1;
                else if (i == j - 1) dp[i][j] = arr[i] == arr[j] ? 1 : 2;
                else {
                    dp[i][j] = arr[i] == arr[j] ? dp[i + 1][j - 1] : j - i + 1;

                    for (int k = i; k < j; k++) {
                        dp[i][j] = Math.min(dp[i][j], dp[i][k] + dp[k + 1][j]);
                    }
                }
            }
        }
        return dp[0][n - 1];
    }
}
```

&emsp;时间复杂度，空间复杂度不变。

执行用时：43ms，在所有java提交中击败了42.39%的用户。

内存消耗：39.7MB，在所有java提交中击败了50.00%的用户。