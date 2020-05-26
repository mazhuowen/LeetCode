[toc]

Given a string `s`, partition s such that every substring of the partition is a palindrome.

Return the minimum cuts needed for a palindrome partitioning of `s`.



## 题目解读

&emsp;给定字符串，求最小的切分次数使得每个子字符串为回文字符串。

```java
class Solution {
    public int minCut(String s) {

    }
}
```

## 程序设计

* 参考社区思路，动态规划数组`dp(i)`表示截至位置`i`的字符串的最小切分次数，则在前`i`个位置中检测，如果存在`j~i`为回文串，则可以判断更新`dp(i) = min(dp(j) + 1)`。
* 判断子串可以最简单的使用中心扩散判断，此处采用曼彻斯特算法提前处理得到所有回文子串的信息。

```java
class Solution {
    public int minCut(String s) {
        if (s == null || s.isEmpty()) return 0;

        char[] str = s.toCharArray();
        int[] P = productPalindrome(str);

        int[] dp = new int[str.length];
        // 最坏的情况，每个字符拆分
        for (int i = 0; i < str.length; i++) {
            dp[i] = i;
        }

        for (int i = 1; i < str.length; i++) {
            // 回文，则无需分割
            if (isPalindrome(0, i, P)) {
                dp[i] = 0;
                continue;
            }

            for (int j = 0; j < i; j++) {
                if (isPalindrome(j + 1, i, P)) dp[i] = Math.min(dp[i], dp[j] + 1);
            }
        }
        return dp[str.length - 1];
    }

    // 曼彻斯特算法生成回文子串长度
    private int[] productPalindrome(char[] str) {
        char[] T = new char[2 * str.length + 3];
        int idx = 0;
        T[idx++] = '^';
        T[idx++] = '#';
        for (char c : str) {
            T[idx++] = c;
            T[idx++] = '#';
        }
        T[idx++] = '$';

        int[] P = new int[T.length];
        int center = 0, right = 0;
        for (int i = 1; i < T.length - 1; i++) {
            if (i < right) P[i] = Math.min(P[2 * center - i], right - i);
            while (T[i - P[i] - 1] == T[i + P[i] + 1]) P[i]++;

            if (i + P[i] > right) {
                center = i;
                right = i + P[i];
            } 
        }
        return P;
    }

    private boolean isPalindrome(int start, int end, int[] P) {
        // 校验回文串中心位置最大长度是否大于当前长度
        int center = 2 + start + end;
        if (P[center] >= end - start + 1) return true;
        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：5ms，在所有java提交中击败了95.29%的用户。

内存消耗：37.4MB，在所有java提交中击败了14.29%的用户。

## 官方解题

&emsp;暂无，密切关注。