[toc]

A **happy string** is a string that:

* consists only of letters of the set `['a', 'b', 'c']`.
* $\text{s[i]} \ne \text{s[i + 1]}$ for all values of $i$ from $1$ to $\text{s.length} - 1$ (string is 1-indexed).

For example, strings "abc", "ac", "b" and "abcbabcbcb" are all happy strings and strings "aa", "baa" and "ababbc" are not happy strings.

Given two integers $n$ and $k$, consider a list of all happy strings of length $n$ sorted in lexicographical order.

Return the kth string of this list or return an **empty string** if there are less than $k$ happy strings of length $n$.



**Constraints:**

- $1 \le n \le 10$
- $1 \le k \le 100$



## 题目解读

&emsp;定义只由`a`、`b`、`c`构成的相邻字符不相等的字符串为开心字符串，求按字典序排列为第$k$个的长度为$n$的字符串。

```java
class Solution {
    public String getHappyString(int n, int k) {
        
    }
}
```

## 程序设计

* 

```java
class Solution {
    public String getHappyString(int n, int k) {
        // k超出所有组合
        if (k > getNum(n)) return "";
        if (n == 1) return "" + (char)('a' + k - 1);

        // 遍历计算首字符
        int num = getNum2(n - 1);
        char c = 'a';
        while (k > num) {
            k -= num;
            c++;
        }
        // 首字符为c
        return c + getHappyString(n - 1, k, c);
    }

    private String getHappyString(int n, int k, char pre) {
        if (n == 1) {
            char c = (char)('a' + k - 1);
            if (c >= pre) c++;
            return "" + c;
        }

        char c = 'a';
        int num = getNum2(n - 1);
        while (k > num) {
            k -= num;
            c++;
        }
        if (c >= pre) c++;
        return c + getHappyString(n - 1, k, c);
    }

    // 计算长度为n的开心字符串组合数目
    private int getNum(int n) {
        return 3 << (n - 1);
    }

    // 计算2^n
    private int getNum2(int n) {
        return 1 << n;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：13ms，在所有java提交中击败了49.25%的用户。

内存消耗：38.4MB，在所有java提交中击败了52.66%的用户。

## 官方解题

&emsp;暂无，密切关注。社区采用直接计算模拟的方式，比递归更快。

```java
class Solution {
    public String getHappyString(int n, int k) {
        // 多于最大组合数目
        if (k > 3 << (n - 1)) return "";

        char[] seq = new char[n];
        // 遍历计算每一位字符
        for (int i = 0; i < n; i++) {
            char c = 'a';
            // 剩余字符串的组合数
            int num = 1 << (n - 1 - i);
            // 如果数目多余当前组合数或字符于前一字符相等，则需迭代到下一个字符
            while (k > num || i > 0 && seq[i - 1] == c) {
                // 于前一字符不相等时，需要减去组合数
                if (i == 0 || i > 0 && seq[i - 1] != c) k -= num;
                c++;
            }
            seq[i] = c;
        }
        return new String(seq);
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户

内存消耗：37.2MB，在所有java提交中击败了95.27%的用户。