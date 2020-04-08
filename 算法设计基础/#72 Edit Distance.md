[toc]

Given two words `word1` and `word2`, find the minimum number of operations required to convert `word1` to `word2`.

You have the following 3 operations permitted on a word:

* Insert a character
* Delete a character
* Replace a character



## 题目解读

&emsp;最小编辑距离。

```java
class Solution {
    public int minDistance(String word1, String word2) {

    }
}
```

## 程序设计

* 基本的最短编辑距离问题。

```java
class Solution {
    public int minDistance(String word1, String word2) {
        if (word1 == null || word2 == null) throw new IllegalArgumentException("invalid param");

        int m = word1.length(), n = word2.length();
        // 动态规划，保存两个字符串相应位置的最小编辑距离
        int[][] dp = new int[m + 1][n + 1];

        // 初始化空串到另一个字符的编辑距离
        for (int i = 0; i <= m; i++) {
            dp[i][0] = i;
        }
        for (int i = 0; i <= n; i++) {
            dp[0][i] = i;
        }
        // 动态规划
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                // word1 0～i到word2 0～j-1，加现在的第j个字符得到0～j（即插入字符j）
                int left = dp[i][j - 1] + 1;
                // 同理
                int down = dp[i - 1][j] + 1;

                int pre = dp[i - 1][j - 1];
                // 如果i和j不相等，还需要加上交换字符i和j的代价
                if (word1.charAt(i - 1) != word2.charAt(j - 1)) pre += 1;

                // 选取这三个操作中最小的
                dp[i][j] = Math.min(pre, Math.min(left, down));
            }
        }

        return dp[m][n];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：6ms，在所有java提交中击败了92.42%的用户。

内存消耗：39.6MB，在所有java提交中击败了5.14%的用户。

## 官方解题

&emsp;同上。