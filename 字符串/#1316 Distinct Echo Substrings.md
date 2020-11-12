[toc]

Return the number of **distinct** non-empty substrings of `text` that can be written as the concatenation of some string with itself (i.e. it can be written as `a + a` where `a` is some string).



**Constraints:**

- $1 \le \text{text.length} \le 2000$
- `text` has only lowercase English letters.



## 题目解读

&emsp;返回给定字符串中所有由`a+a`模式组成的子字符串数目。

```java
class Solution {
    public int distinctEchoSubstrings(String text) {

    }
}
```

## 程序设计

* 如果使用暴力匹配，时间复杂度为$O(N^3)$，代价较大；转换思路，采用`RK`算法比较前后两个等长子字符串是否相等。

```java
class Solution {
    private static final int MOD = 1_000_000_007;

    public int distinctEchoSubstrings(String text) {
        if (text == null || text.isEmpty()) return 0;
        char[] seq = text.toCharArray();

        int res = 0;
        // 表示起始位置为i的子字符串的哈希值
        int[] hash = new int[seq.length];

        for (int len = 1; len <= seq.length / 2; len++) {
            Set<Integer> set = new HashSet<>();
            int[] tmp = new int[seq.length];
            for (int i = 0; i + len <= seq.length; i++) {
                // 在原来长度为len - 1的字符串基础上增加i + len - 1位置的字符
                long cur = (hash[i] * 26L + (seq[i + len - 1] - 'a')) % MOD;
                tmp[i] = (int)cur;
                // 前后相等
                if (i - len >= 0 && tmp[i - len] == tmp[i] && equal(seq, i, i - len, len)) set.add(tmp[i]);
            }
            res += set.size();
            hash = tmp;
        }
        return res;
    }

    private boolean equal(char[] seq, int i, int j, int len) {
        for (int l = 0; l < len; l++) {
            if (seq[i + l] != seq[j + l]) return false;
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：612ms，在所有java提交中击败了61.63%的用户。

内存消耗：38.7MB，在所有java提交中击败了98.41%的用户。

## 官方解题

&emsp;官方采用一次计算好哈希值，通过类似前缀和的思路进行计算。

```java
class Solution {
    private static final long BASE = 31L;
    private static final int MOD = 1_000_000_007;

    public int distinctEchoSubstrings(String text) {
        if (text == null || text.isEmpty()) return 0;
        char[] seq = text.toCharArray();
        // 计算哈希值和BASE的i次方
        int[] hash = new int[seq.length + 1];
        long[] val = new long[seq.length + 1];
        val[0] = 1L;
        for (int i = 1; i <= seq.length; i++) {
            hash[i] = (int)((hash[i - 1] * BASE + (seq[i - 1] - 'a')) % MOD);
            val[i] = val[i - 1] * BASE % MOD;
        }

        int res = 0;
        for (int len = 1; len <= seq.length / 2; len++) {
            Set<Integer> set = new HashSet<>();
            for (int i = len; i + len <= seq.length; i++) {
                int cur = getHash(hash, val, i, i + len - 1);
                if (cur == getHash(hash, val, i - len, i - 1)) set.add(cur);
            }
            res += set.size();
        }
        
        return res;
    }

    private int getHash(int[] hash, long[] val, int i, int j) {
        return (int)((hash[j + 1] - hash[i] * val[j - i + 1] % MOD + MOD) % MOD);
    }
}
```

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：237ms，在所有java提交中击败了79.07%的用户。

内存消耗：39.1MB，在所有java提交中击败了79.37%的用户。