[toc]

Given a **non-empty** string, encode the string such that its encoded length is the shortest.

The encoding rule is: `k[encoded_string]`, where the encoded_string inside the square brackets is being repeated exactly k times.

Note:

* k will be a positive integer and encoded string will not be empty or have extra space.
* You may assume that the input string contains only lowercase English letters. The string's length is at most 160.
* If an encoding process does not make the string shorter, then do not encode it. If there are several solutions, return any of them is fine.



## 题目解读

&emsp;给定字符串，判断其最短编码。

```java
class Solution {
    public String encode(String s) {

    }
}
```

## 程序设计

* 首先想到的是暴力回溯，关键难点在于判断字符串是否存在重复子串。参考社区思路，可以将该字符串与自己拼接，然后在拼接后的字符串在位置0后查找自己。以`abc`为例，拼接后为`abcabc`，查找后`idx=3`，与字符串长度一致，表明不存在重复子串；而`aabaab`拼接后为`aabaabaabaab`，查找可得`idx=3`，小于字符串长度，表明存在重复子串，且为0~2的序列，即`aab`。
* 有了这个巧妙的思路。每次回溯都判断是否重复，重复则根据规则拼接并继续判断子串；然后需要尝试当前字符串的所有拼接方式，取最短的字符串。粗略判断，时间复杂度为$O(3^N)$，会超时。

```java
class Solution {
    public String encode(String s) {
        if (s == null) throw new IllegalArgumentException("invalid param");
        // 短于4，返回原串
        if (s.length() <= 4) return s;

        String res = s;

        int idx = (s + s).indexOf(s, 1);
        // 存在重复子串
        if (idx < s.length()) {
            // 递归遍历子串
            res = s.length() / idx + "[" + encode(s.substring(0, idx)) + "]";
        }
        // 尝试所有切分方式，i表示包含i及之前的序列切分方式
        for (int i = 1; i < s.length() - 1; i++) {
            String left = encode(s.substring(0, i + 1));
            String right = encode(s.substring(i + 1));
            res = res.length() < (left + right).length() ? res : (left + right);
        }
        return res;
    }
}
```

* 上述回溯思路会重复尝试，由于方法返回的是区间内最短缩写，可引入额外空间保存计算结果，避免重复计算。

```java
class Solution {
    public String encode(String s) {
        if (s == null) throw new IllegalArgumentException("invalid param");

        int len = s.length();
        String[][] record = new String[len][len];

        return encode(s, 0, len - 1, record);
    }

    private String encode(String s, int start, int end, String[][] record) {
        if (record[start][end] != null) return record[start][end];

        String res = s.substring(start, end + 1);
        // 短于4个，直接返回字符串
        if (end - start < 4) {
            record[start][end] = res;
            return res;
        }

        // 表明有重复字符串
        int idx = (res + res).indexOf(res, 1);
        if (idx <= end - start) {
            res = (end - start + 1) / idx + "[" + encode(s, start, start + idx - 1, record) + "]";
        }
        // 尝试所有区间的取值，取最短的
        for (int i = start; i < end; i++) {
            String temp = encode(s, start, i, record) + encode(s, i + 1, end, record);
            res = res.length() > temp.length() ? temp : res;
        }
        record[start][end] = res;
        return res;
    }
}
```

* 将上述回溯算法改为动态规划算法：

```java
class Solution {
    public String encode(String s) {
        if (s == null) throw new IllegalArgumentException("invalid param");

        int n = s.length();
        String[][] dp = new String[n][n];

        // 从长度为1一直更新到长度为n
        for (int len = 0; len < n; len++) {
            // 起始和结束位置
            for (int start = 0, end = start + len; end < n; start++, end++) {
                // 短于4个
                if (end - start < 4) dp[start][end] = s.substring(start, end + 1);
                else {
                    // 判断重复子串
                    String temp = s.substring(start, end + 1);
                    int idx = (temp + temp).indexOf(temp, 1);
                    if (idx <= end - start) {
                        dp[start][end] = (end - start + 1) / idx + "[" + dp[start][start + idx - 1] + "]";
                    }
                    // 遍历所有组合
                    for (int i = start; i < end; i++) {
                        temp = dp[start][i] + dp[i + 1][end];
                        dp[start][end] = dp[start][end] == null || dp[start][end].length() > temp.length() ? temp : dp[start][end];
                    }
                }
            }
        }
        return dp[0][n - 1];
    }
}
```

## 性能分析

&emsp;优化的回溯法时间复杂度为$O(2^N)$，空间复杂度为$O(N^2)$。

执行用时：96ms，在所有java提交中击败了23.38%的用户。

内存消耗：41.6MB，在所有java提交中击败了100.00%的用户。

&emsp;动态规划时间复杂度为$O(N^3)$，空间复杂度为$O(N^2)$。

执行用时：63ms，在所有java提交中击败了24.68%的用户。

内存消耗：41.7MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区时间性能较好方法思路大体相同，实现略有差异。