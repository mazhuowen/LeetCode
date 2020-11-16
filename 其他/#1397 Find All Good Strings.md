[toc]

Given the strings `s1` and `s2` of size $n$, and the string `evil`. Return the number of **good** strings.

A **good** string has size $n$, it is alphabetically greater than or equal to `s1`, it is alphabetically smaller than or equal to `s2`, and it does not contain the string `evil` as a substring. Since the answer can be a huge number, return this modulo $10^9 + 7$.



**Constraints**:

* $\text{s1.length} == n$
* $\text{s2.length} == n$
* $s_1 \le s_2$
* $1 \le n \le 500$
* $1 \le \text{evil.length} \le 50$
* All strings consist of lowercase English letters.



## 题目解读

&emsp;好字符串定义为长度为$n$，字典序大于等于`s1`，小于等于`s2`，且不包含`evil`得字符串。求给定`s1`和`s2`间得所有好字符数目。

```java
class Solution {
    public int findGoodStrings(int n, String s1, String s2, String evil) {

    }
}
```

## 程序设计

* 由于是求`s1`和`s2`区间内的符合要求的字符串，首先想到的是数位动态规划，即`pos[pos][stat][lead][limit]`，由于本题要求字符串长度为$n$，`lead`参数可以去掉，现在终点在于`stat`和`limit`的取舍。
* `limit`由于是上下界，通常单独上界的$0$、$1$无法表达下界，为此设置为四个状态，$0$表示在上下界之间，$1$表示是下界，$2$表示是上界，$3$表示是上下界。
* 根据本题不能包含`evil`字符串，`stat`可设置为之前的字符串（长度不超过`evil`），但是如何将字符串表达为数字是个难题，使用`RK`算法会遇到数值过大哈希碰撞的问题。此处参考官方解题，状态可以表示为当前字符串（即回溯尝试的字符串）匹配`evil`字符串（即模式串）的位置；则每次尝试字符`c`，在上次匹配的位置继续匹配，返回当前匹配到的位置，如果是模式串的串尾，说明匹配到`evil`字符串，不符合要求，不再尝试。

```java
class Solution {
    private static final int MOD = 1_000_000_007;

    String s1, s2, evil;
    private int[] next;
    // 主串模式串匹配状态
    private int[][] trans;
    // 动态规划数组
    int[][][] dp;

    public int findGoodStrings(int n, String s1, String s2, String evil) {
        this.s1 = s1;
        this.s2 = s2;
        this.evil = evil;
        this.next = initNext(evil);
        this.trans = new int[evil.length()][26];
        this.dp = new int[n][evil.length()][4];

        // 初始化为-1
        for (int[] array : trans) Arrays.fill(array, -1);
        for (int[][] array : dp) {
            for (int[] arr : array) Arrays.fill(arr, -1);
        }

        return backTracing(0, 0, 3);
    }

    // KMP next数组
    private int[] initNext(String pattern) {
        int k = -1, n = pattern.length();
        int[] next = new int[n];
        next[0] = k;

        for (int i = 1; i < n; i++) {
            while (k != -1 && pattern.charAt(i) != pattern.charAt(k + 1)) k = next[k];
            if (pattern.charAt(i) == pattern.charAt(k + 1)) k++;
            next[i] = k;
        }
        return next;
    }

    // 生成状态转化数组，即与模式串匹配的第i个位置字符为c时的情况
    private int tranStat(int i, char c) {
        if (trans[i][c - 'a'] != -1) return trans[i][c - 'a'];

        // 主串匹配模式串，即c匹配模式串i位置
        while (i > 0 && evil.charAt(i) != c) i = next[i - 1] + 1;
        // 匹配到，则当前索引与字符c对应的下一索引为i+1
        if (evil.charAt(i) == c) return trans[i][c - 'a'] = ++i;
        // 未匹配到，模式串从头匹配，索引为0
        return trans[i][c - 'a'] = 0;
    }

    // 记忆化回溯
    private int backTracing(int pos, int stat, int limit) {
        // 匹配到evil字符串
        if (stat == evil.length()) return 0;
        // 匹配结束
        if (pos >= dp.length) return 1;
        if (dp[pos][stat][limit] != -1) return dp[pos][stat][limit];

        int res = 0;
        // 根据边界确定上下界，即1,2,3
        char low = (limit & 1) == 1 ? s1.charAt(pos) : 'a';
        char high = (limit & 2) == 2 ? s2.charAt(pos) : 'z';
        // 尝试
        for (char c = low; c <= high; c++) {
            int nextStat = tranStat(stat, c);
            int nextLimit = ((limit & 1) == 1 && c == low ? 1 : 0) ^ ((limit & 2) == 2 && c == high ? 2 : 0);
            res = (res + backTracing(pos + 1, nextStat, nextLimit)) % MOD;
        }
        return dp[pos][stat][limit] = res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(26MN)$，空间复杂度为$O(MN)$。

执行用时：27ms，在所有java提交中击败了68.42%的用户。

内存消耗：39.8MB，在所有java提交中击败了25.00%的用户。

## 官方解题

&emsp;上述思路参考官方。