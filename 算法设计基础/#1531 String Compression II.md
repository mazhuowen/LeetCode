[toc]

`Run-length encoding` is a string compression method that works by replacing consecutive identical characters (repeated 2 or more times) with the concatenation of the character and the number marking the count of the characters (length of the run). For example, to compress the string `"aabccc"` we replace `"aa"` by `"a2"` and replace `"ccc"` by `"c3"`. Thus the compressed string becomes `"a2bc3"`.

Notice that in this problem, we are not adding `'1'` after single characters.

Given a string `s` and an integer `k`. You need to delete at most `k` characters from `s` such that the `run-length encoded` version of `s` has minimum length.

Find the minimum length of the run-length encoded version of `s` after deleting at most `k` characters.



**Constraints**:

* $1 \le \text{s.length} \le 100$
* $0 \le k \le \text{s.length}$
* `s` contains only lowercase English letters.



## 题目解读

&emsp;求最多删减$k$个字符后的最短行程长度编码。

```java
class Solution {
    public int getLengthOfOptimalCompression(String s, int k) {
    
    }
}
```

## 程序设计

* 参考社区思路，`dp(i,j)`表示前$i$个字符删除$j$个字符后的最短缩写，则有如下情况：
  * 删除位置$i$的字符，则`dp(i,j) = dp(i-1,j-1)`；
  * 删除$i$之前的若干个与位置$i$字符不相等的字符，使得$i$与前面中相等字符连成一片，假设删除`del`个，则`dp(i,j) = dp(l-1,j-del)+newLen`。

```java
class Solution {
    public int getLengthOfOptimalCompression(String s, int k) {
        if (s == null || s.isEmpty()) return 0;

        int n = s.length();
        char[] seq = s.toCharArray();
        // 动态规划数组dp(i,j)表示前i个字符串删除j个字符后的最小缩写
        int[][] dp = new int[n + 1][k + 1];
        for (int[] array : dp) {
            Arrays.fill(array, Integer.MAX_VALUE >> 1);
        }
        dp[0][0] = 0;
        
        for (int i = 1; i <= n; i++) {
            for (int j = 0; j <= k; j++) {
                // 第一种情况将i-1位置的字符删除，长度与dp(i-1,j-1)相等
                if (j > 0) dp[i][j] = dp[i - 1][j - 1];
                // 第二种情况保留i-1位置的字符及之前的相等字符，删除中间的不相等字符
                int keep = 0, del = 0;
                // 此处l为dp中索引
                for (int l = i; l >= 1; l--) {
                    if (seq[l - 1] == seq[i - 1]) keep++;
                    else del++;
                    // 删除合并l~i段，拼接l-1段的长度（此处包含了k=0的情况）
                    if (j - del >= 0) dp[i][j] = Math.min(dp[i][j], dp[l - 1][j - del] + getLen(keep));
                }
            }
        }

        int min = Integer.MAX_VALUE;
        for (int j = 0; j <= k; j++) {
            min = Math.min(min, dp[n][j]);
        }
        return min;
    }

    private int getLen(int count) {
        int len = 1;
        if (count == 1);
        else if (count < 10) len += 1;
        else if (count < 100) len += 2;
        else len += 3;
        return len;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2K)$，空间复杂度为$O(NK)$。

执行用时：52ms，在所有java提交中击败了71.43%的用户。

内存消耗：39.4MB，在所有java提交中击败了93.33%的用户。

## 官方解题

&emsp;暂无，密切关注。