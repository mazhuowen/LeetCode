[toc]

Given a string, your task is to count how many palindromic substrings in this string.

The substrings with different start indexes or end indexes are counted as different substrings even they consist of same characters.



**Note:**

The input string length won't exceed $1000$.



## 题目解读

&emsp;统计回文子串的数目。

```java
class Solution {
    public int countSubstrings(String s) {

    }
}
```

## 程序设计

* 中心扩展时计数。

```java
class Solution {
    public int countSubstrings(String s) {
        if (s == null || s.length() == 0) return 0;

        char[] str = s.toCharArray();

        int count = 0;
        for (int i = 0; i < str.length; i++) {
            // 中心扩展
            int left = i, right = i;
            while (left >= 0 && right < str.length && str[left] == str[right]) {
                count++;
                left--;
                right++;
            }

            if (i + 1 < str.length && str[i] == str[i + 1]) {
                left = i;
                right = i + 1;
                while (left >= 0 && right < str.length && str[left] == str[right]) {
                    count++;
                    left--;
                    right++;
                }
            }
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.1MB，在所有java提交中击败了5.55%的用户。

## 官方解题

&emsp;官方还提供了最优的曼彻斯特算法。

```java
class Solution {
    public int countSubstrings(String S) {
        // 插入#，其中头尾各用特殊符号标记，头部为@#，尾部为$
        // 加入#使得长度为偶数的回文化作奇数的情况
        char[] A = new char[2 * S.length() + 3];
        A[0] = '@';
        A[1] = '#';
        A[A.length - 1] = '$';
        int t = 2;
        for (char c: S.toCharArray()) {
            A[t++] = c;
            A[t++] = '#';
        }

        int[] Z = new int[A.length];
        int center = 0, right = 0;
        for (int i = 1; i < Z.length - 1; ++i) {
            if (i < right)
                Z[i] = Math.min(right - i, Z[2 * center - i]);
            while (A[i + Z[i] + 1] == A[i - Z[i] - 1])
                Z[i]++;
            if (i + Z[i] > right) {
                center = i;
                right = i + Z[i];
            }
        }
        int ans = 0;
        for (int v: Z) ans += (v + 1) / 2;
        return ans;
    }
}
```

