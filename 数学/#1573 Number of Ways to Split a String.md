[toc]

Given a binary string `s` (a string consisting only of '0's and '1's), we can split s into 3 **non-empty** strings s1, s2, s3 (s1+ s2+ s3 = s).

Return the number of ways `s` can be split such that the number of characters '1' is the same in s1, s2, and s3.

Since the answer may be too large, return it modulo $10^9 + 7$.



**Constraints:**

- `s[i] == '0'` or `s[i] == '1'`
- $3 \le \text{s.length} \le 10^5$



## 题目解读

&emsp;给定二进制字符串，求将字符串切分为三份的方案数，要求每一份包含的字符`1`的数目相等。

```java
class Solution {
    public int numWays(String s) {

    }
}
```

## 程序设计

* 首先统计字符串中`1`的数目是否可三等分，不能返回$0$；如果可以三等分，分如下情况：
  * 有$0$个`1`，即字符串都由`0`构成，假设长度为$n$，则目标就是在$n - 1$个间隙中找到两个切分点，即$C_n^2$；
  * 假设恰好满足划分个数的子字符串（即首尾为`1`，总共包含规定的`1`的数目）分别为`a`、`b`、`c`，则字符串构成为`0…0a0…0b0…0c…0`，由于是三等分，首尾的`0`都会划分到`a`、`c`中，问题在于`a`于`b`之间、`b`于`c`之间的`0`的序列，假设长度分别为$l$、$m$，则共有$(l + 1) * (m + 1)$种切分方案。

```java
class Solution {
    private static final int MOD = 1_000_000_007;
    
    public int numWays(String s) {
        if (s == null || s.length() < 3) return 0;
        char[] seq = s.toCharArray();
        // 计数
        int counter = 0;
        for (char c : seq) if (c == '1') counter++;
        if (counter % 3 != 0) return 0;
        
        counter /= 3;
        long res;
        // 全部是0
        if (counter == 0) res = ((long)seq.length - 1) * (seq.length - 2) / 2 % MOD;
        // 划分点
        else {
            res = 1;
            int idx = 0, count = 0;
            // 遍历划分包含要求的1的序列，并统计其后0的数目
            while (idx < seq.length) {
                if (seq[idx++] == '1') count++;
                // 统计其后0的数目
                if (count == counter) {
                    int len = 1;
                    while (idx < seq.length && seq[idx] == '0') {
                        idx++;
                        len++;
                    }
                    if (idx < seq.length) res = (res * len) % MOD;
                    count = 0;
                }
            }
        }
        return (int)res;
    }
    
}
```

* 上述思路还需要遍历第二次切分字符串，事实上可以在第一次就记录每个`1`的索引，引入数组加速运算：

```java
class Solution {
    private static final int MOD = 1_000_000_007;
    
    public int numWays(String s) {
        if (s == null || s.length() < 3) return 0;
        char[] seq = s.toCharArray();
        // 记录1的索引
        int[] index = new int[seq.length];
        // 计数
        int counter = 0;
        for (int i = 0; i < seq.length; i++) if (seq[i] == '1') index[counter++] = i;
        if (counter % 3 != 0) return 0;
        
        long res;
        // 全部是0
        if (counter == 0) res = ((long)seq.length - 1) * (seq.length - 2) / 2 % MOD;
        // 划分点
        else {
           int idx1 = index[counter / 3 - 1], idx2 = index[counter / 3], idx3 = index[2 * counter / 3 - 1], idx4 = index[2 * counter / 3];
           int len1 = idx2 - idx1, len2 = idx4 - idx3;
           res = (long)len1 * len2 % MOD;
        }
        return (int)res;
    }
    
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：6ms，在所有java提交中击败了99.56%的用户。

内存消耗：40.3MB，在所有java提交中击败了62.48%的用户。

## 官方解题

&emsp;暂无，密切关注。