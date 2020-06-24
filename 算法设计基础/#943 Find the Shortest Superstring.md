[toc]

Given an array `A` of strings, find any smallest string that contains each string in `A` as a substring.

We may assume that no string in `A` is substring of another string in `A`.



**Note**:

* $1 \le \text{A.length} \le 12$
* $1 \le \text{A[i].length} \le 20$



## 题目解读

&emsp;给定字符串数组，得出最短超级字符串，超级字符串表示包含所有字符串的字符串。

```java
class Solution {
    public String shortestSuperstring(String[] A) {

    }
}
```

## 程序设计

* 最基本的思路是暴力回溯，遍历当前字符串，如果当前字符串与已拼接字符串结尾有重合，则截取拼接；最后选择最短的字符串。时间复杂度为$O(N!M)$，$M$为字符串平均长度，会超时。

```java
class Solution {
    String minStr;

    public String shortestSuperstring(String[] A) {
        if (A == null || A.length == 0) return "";

        boolean[] visited = new boolean[A.length];
        for (int i = 0; i < A.length; i++) {
            superstring(A, i, visited, new StringBuffer());
        }
        return minStr;
    }

    private void superstring(String[] A, int idx, boolean[] visited, StringBuffer sb) {
        if (minStr != null && sb.length() >= minStr.length()) return;

        visited[idx] = true;
        int len = append(sb, A[idx]);

        boolean flag = true;
        // 试探
        for (int i = 0; i < A.length; i++) {
            if (visited[i]) continue;

            flag = false;
            superstring(A, i, visited, sb);
        }

        if (flag && (minStr == null || minStr.length() > sb.length())) minStr = sb.toString();
        
        // 回溯
        visited[idx] = false;
        sb.setLength(sb.length() - len);
    }

    private int append(StringBuffer sb, String str) {
        String temp = sb.toString();
        int idx = 0, len = 0;
        while (idx < str.length()) {
            if (temp.endsWith(str.substring(0, idx + 1))) len = idx + 1;
            idx++;
        }
        sb.append(str.substring(len));
        return str.length() - len;
    }
}
```

* 如果把回溯中的访问状态压缩为二进制数字，每次拼接都只关当前字符和上一个字符，可用动态规划数组`dp(i,j)`表示路径为`i`，结尾字符串为$j$时的最短字符串。
* 得到动态规划数组后可知最终最短字符串的结尾字符串，然后可倒推出整个序列。

```java
class Solution {

    public String shortestSuperstring(String[] A) {
        if (A == null || A.length == 0) return "";

        int n = A.length;
        int[][] overland = overland(A);
        // 动态规划数组表示路径为i，结尾字符串为j的最短长度
        int[][] dp = new int[1 << n][n];
        // 记录路径
        int[][] parent = new int[1 << n][n];

        for (int path = 1; path < (1 << n); path++) {
            for (int i = 0; i < n; i++) {
                // 当前路径不存在第i个字符串，跳过
                if (((path >> i) & 1) == 0) continue;

                // 递归更新
                int prePath = path ^ (1 << i);
                // 首字符串，长度为自身
                if (prePath == 0) {
                    dp[path][i] = A[i].length();
                    parent[path][i] = -1;
                    continue;
                }

                for (int j = 0; j < n; j++) {
                    if (((prePath >> j) & 1) == 0) continue;

                    // 前一序列以j结尾，加上当前的i构成当前路径
                    if (dp[path][i] == 0 || dp[path][i] > dp[prePath][j] + A[i].length() - overland[j][i]) {
                        dp[path][i] = dp[prePath][j] + A[i].length() - overland[j][i];
                        parent[path][i] = j;
                    }
                }
            }
        }

        // 最短长度对应的结尾字符串
        int cur = 0, path = (1 << n) - 1;
        for (int i = 1; i < n; i++) {
            if (dp[path][i] < dp[path][cur]) cur = i;
        }

        // 拼接
        StringBuffer sb = new StringBuffer();
        while (path > 0) {
            int pre = parent[path][cur];
            int overLen = pre == -1 ? 0 : overland[pre][cur];
            sb.insert(0, A[cur].substring(overLen));
            path = path ^ (1 << cur);
            cur = pre;
        }

        return sb.toString();
    }

    // 提前计算覆盖长度
    private int[][] overland(String[] strs) {
        int[][] overland = new int[strs.length][strs.length];

        for (int i = 0; i < strs.length; i++) {
            for (int j = 0; j < strs.length; j++) {
                if (i == j) overland[i][j] = strs.length;
                overland[i][j] = overland(strs[i], strs[j]);
            }
        }
        return overland;
    }

    // 返回字符串一包含的字符串二最大长度
    private int overland(String str1, String str2) {
        int len = Math.min(str1.length(), str2.length());

        for (int i = len; i >= 1; i--) {
            if (str1.endsWith(str2.substring(0, i))) return i;
        }
        return 0;
    }
}
```

## 性能分析

&emsp;计算覆盖长度时间复杂度为$O(N^2)$，动态规划时间复杂度为$O(N^22^N)$，空间复杂度为$O(N2^N)$。

执行用时：28ms，在所有java提交中击败了58.75%的用户。

内存消耗：40.1MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;上述思路参考官方解题。