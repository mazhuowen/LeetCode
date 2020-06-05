[toc]

Given an array, `strs`, with strings consisting of only `0s` and `1s`. Also two integers `m` and `n`.

Now your task is to find the maximum number of strings that you can form with given **m** `0s` and **n** `1s`. Each `0` and `1` can be used at most once.



Constraints:

* $1 \le \text{strs.length} \le 600$
* $1 \le \text{strs[i].length} \le 100$
* `strs[i]` consists only of digits `'0'` and `'1'`.
* $1 \le m, n \le 100$



## 题目解读

&emsp;给定一定数量的0和1及由0、1构成的字符串列表，求可由这些0、1构成的最大字符串数目。

```java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {

    }
}
```

## 程序设计

* 如果不考虑1，假设字符串都由0构成，则问题转化为容量为`M`的背包在`N`个字符串中可装入的最大数量，此处物品的价值就是数目1。
* 考虑1，问题转化为字符串数组可放入容量为`N`，`M`的最大数目，即是二维背包问题。

```java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        if (strs == null || strs.length == 0) return 0;

        int len = strs.length;
        // dp(i,j,k)表示前i个字符串可装在j、ｋ两个背包的最大数目，j为0的背包，k为1的背包
        int[][][] dp = new int[len + 1][m + 1][n + 1];
        for (int i = 1; i <= len; i++) {
            String curStr = strs[i - 1];
            int zero = countZeroNum(curStr.toCharArray());
            int one = curStr.length() - zero;
            for (int j = 0; j <= m; j++) {
                for (int k = 0; k <= n; k++) {
                    if (j < zero || k < one) dp[i][j][k] = dp[i - 1][j][k];
                        // 选择不加入当前字符串及加入当前字符串两种方案中较大的那种
                    else dp[i][j][k] = Math.max(dp[i - 1][j][k], dp[i - 1][j - zero][k - one] + 1);
                }
            }
        }
        return dp[len][m][n];
    }

    private int countZeroNum(char[] strs) {
        int count = 0;
        for (char c : strs) {
            if (c == '0') count++;
        }
        return count;
    }
}
```

* 优化空间：

```java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        if (strs == null || strs.length == 0) return 0;

        int[][] dp = new int[m + 1][n + 1];
        for (String str : strs) {
            int zero = countZeroNum(str.toCharArray());
            int one = str.length() - zero;
            for (int j = m; j >= zero; j--) {
                for (int k = n; k >= one; k--) {
                    // 选择不加入当前字符串及加入当前字符串两种方案中较大的那种
                    dp[j][k] = Math.max(dp[j][k], dp[j - zero][k - one] + 1);
                }
            }
        }
        return dp[m][n];
    }
    
    private int countZeroNum(char[] strs) {
        int count = 0;
        for (char c : strs) {
            if (c == '0') count++;
        }
        return count;
    }
}
```

## 性能分析

&emsp;原始动态规划时间复杂度为$O(Len * N * M)$，空间复杂度为$O(Len * N * M)$。

执行用时：68ms，在所有java提交中击败了23.98%的用户。

内存消耗：68.7MB，在所有java提交中击败了16.67%的用户。

&emsp;空间优化后时间复杂度为$O(Len * N * M)$，空间复杂度为$O(N * M)$。

执行用时：33ms，在所有java提交中击败了76.02%的用户。

内存消耗：39.3MB，在所有java提交中击败了16.67%的用户。

## 官方解题

&emsp;同上。