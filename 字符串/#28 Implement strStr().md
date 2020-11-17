[toc]

Implement `strStr()`.

Return the index of the first occurrence of needle in haystack, or **-1** if needle is not part of haystack.



**Clarification**:

What should we return when `needle` is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return 0 when `needle` is an empty string. This is consistent to C's `strstr()` and Java's `indexOf()`.



## 题目解读

&emsp;查找子串是否出现在字符串中，没有则返回`-1`，否则返回索引。

```java
class Solution {
    public int strStr(String haystack, String needle) {

    }
}
```

## 程序设计

* 最简单的想法字符串匹配的BF算法，即从头遍历，每次截取后面的子串长度的字符串进行比较。

```java
class Solution {
    public int strStr(String haystack, String needle) {
        // 子串为空串
        if (needle == null || needle.isEmpty()) return 0;
        // 字符串为空或长度小于子串
        if (haystack == null || haystack.isEmpty() || haystack.length() < needle.length()) return -1;

        // 遍历判断是否相等
        int m = haystack.length(), n = needle.length();
        for (int i = 0; i <= m - n; i++) {
            if (haystack.substring(i, i + n).equals(needle)) return i;
        }
        return -1;
    }
}
```

* 进阶的BM算法如下：

```java
class Solution {
    public int strStr(String haystack, String needle) {
        if (haystack == null || needle == null || needle.length() > haystack.length()) return -1;
        if (needle.isEmpty()) return 0;
        return bm(haystack, needle);
    }

    private int bm(String source, String pattern) {
        int n = source.length(), m = pattern.length();
        int[] bc = new int[26];
        initBc(bc, pattern, m);

        int[] suffix = new int[m];
        boolean[] prefix = new boolean[m];
        initGs(suffix, prefix, pattern, m);

        int i = 0;
        while (i <= n - m) {
            int j = m - 1;
            while (j >= 0 && source.charAt(i + j) == pattern.charAt(j)) {
                j--;
            }
            if (j == -1) return i;
            int offset = Math.max(0, j - bc[source.charAt(i + j) - 'a']);

            if (j < m - 1) {
                offset = Math.max(offset, offsetByGs(j, m, suffix, prefix));
            }
            i += offset;
        }
        return -1;
    }

    private void initBc(int[] bc, String pattern, int m) {
        Arrays.fill(bc, -1);
        for (int i = 0; i < m; i++) {
            bc[pattern.charAt(i) - 'a'] = i;
        }
    }

    private void initGs(int[] suffix, boolean[] prefix, String pattern, int m) {
        Arrays.fill(suffix, -1);
        for (int i = 0; i < m - 1; i++) {
            int j = i, k = 0;
            while (j >= 0 && pattern.charAt(j) == pattern.charAt(m - 1 - k)) {
                suffix[++k] = j--;
            }
            if (j == -1) prefix[k] = true; 
        }
    }

    private int offsetByGs(int j, int m, int[] suffix, boolean[] prefix) {
        int k = m - j - 1;
        if (suffix[k] != -1) return j - suffix[k] + 1;
        for (int r = j + 2; r < m; r++) {
            if (prefix[m - r]) return r;
        }
        return m;
    }
}
```

* KMP算法如下：

```java
class Solution {
    public int strStr(String haystack, String needle) {
        if (haystack == null || needle == null || needle.length() > haystack.length()) return -1;
        if (needle.isEmpty()) return 0;
        return kmp(haystack, needle);
    }

    private int kmp(String source, String pattern) {
        int n = source.length(), m = pattern.length();
        int[] next = new int[m];
        initNext(next, pattern, m);

        int j = 0;
        for (int i = 0; i < n; i++) {
            while (j > 0 && source.charAt(i) != pattern.charAt(j)) {
                j = next[j - 1] + 1;
            }
            if (source.charAt(i) == pattern.charAt(j)) j++;
            if (j == m) return i - m + 1;
        }
        return -1;
    }

    private void initNext(int[] next, String pattern, int m) {
        next[0] = -1;
        int k = -1;
        for (int i = 1; i < m; i++) {
            while (k != -1 && pattern.charAt(k + 1) != pattern.charAt(i)) {
                k = next[k];
            }
            if (pattern.charAt(k + 1) == pattern.charAt(i)) k++;
            next[i] = k;
        }
    }
}
```

## 性能分析

&emsp;BF算法时间复杂度为$O((N - M)M)$，空间复杂度为$O(1)$。其中$M$是子串的长度，$N$为字符串的长度。

执行用时：1ms，在所有java提交中击败了78.55%的用户。

内存消耗：39.8MB，在所有java提交中击败了5.06%的用户。

&emsp;BM算法时间复杂度为$O(N)$，空间复杂度为$O(M)$。

执行用时：4ms，在所有java提交中击败了34.00% 的用户。

内存消耗：39.7MB，在所有java提交中击败了5.43%的用户。

&emsp;KMP算法时间复杂度为$O(N + M)$，空间复杂度为$O(M)$。

执行用时：8ms，在所有java提交中击败了15.45% 的用户。

内存消耗：38.1MB，在所有java提交中击败了5.43%的用户。

## 官方解题

&emsp;除了上述思路，官方还提供了RK算法实现：

```java
class Solution {
  // function to convert character to integer
  public int charToInt(int idx, String s) {
    return (int)s.charAt(idx) - (int)'a';
  }

  public int strStr(String haystack, String needle) {
    int L = needle.length(), n = haystack.length();
    if (L > n) return -1;

    // base value for the rolling hash function
    int a = 26;
    // modulus value for the rolling hash function to avoid overflow
    long modulus = (long)Math.pow(2, 31);

    // compute the hash of strings haystack[:L], needle[:L]
    long h = 0, ref_h = 0;
    for (int i = 0; i < L; ++i) {
      h = (h * a + charToInt(i, haystack)) % modulus;
      ref_h = (ref_h * a + charToInt(i, needle)) % modulus;
    }
    if (h == ref_h) return 0;

    // const value to be used often : a**L % modulus
    long aL = 1;
    for (int i = 1; i <= L; ++i) aL = (aL * a) % modulus;

    for (int start = 1; start < n - L + 1; ++start) {
      // compute rolling hash in O(1) time
      h = (h * a - charToInt(start - 1, haystack) * aL
              + charToInt(start + L - 1, haystack)) % modulus;
      if (h == ref_h) return start;
    }
    return -1;
  }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：4ms，在所有java提交中击败了34.00% 的用户。

内存消耗：37.9MB，在所有java提交中击败了14.13%的用户。