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
    public int countSubstrings(String s) {
        char[] T = new char[2 * s.length() + 3];
        int idx = 0;
        T[idx++] = '^';
        T[idx++] = '#';
        for (char c : s.toCharArray()) {
            T[idx++] = c;
            T[idx++] = '#';
        }
        T[idx++] = '$';

        int[] P = new int[T.length];
        int center = 0, right = 0;
        int count = 0;
        for (int i = 1; i < T.length - 1; i++) {
            if (i < right) P[i] = Math.min(right - i, P[2 * center - i]);

            while (T[i - P[i] - 1] == T[i + P[i] + 1]) {
                P[i]++;
            }
            // 当前字符为中心的回文串数目为当前最大长度加一除以2
            count += (1 + P[i]) / 2;
            
            if (i + P[i] > right) {
                center = i;
                right = i + P[i];
            }
        }
        return count;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了96.79%的用户。

内存消耗：37.8MB，在所有java提交中击败了5.55%的用户。