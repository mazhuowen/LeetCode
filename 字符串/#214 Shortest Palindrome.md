[toc]

Given a string `s`, you are allowed to convert it to a palindrome by adding characters in front of it. Find and return the shortest palindrome you can find by performing this transformation.



## 题目解读

&emsp;给一个字符串前面添加字符使之成为回文字符串，返回最短的回文字符串。

```java
class Solution {
    public String shortestPalindrome(String s) {

    }
}
```

## 程序设计

* 仔细观察，最短回文串是原字符串`S`除去以左侧字符为起始的最大回文串`T`后剩余的字符串`L`翻转拼接在`S`前形成的字符串。问题变为求解左侧最长回文子串的问题。
* 可使用曼彻斯特算法在遍历过程中记录判断中心是否覆盖左侧第一个字符。

```java
class Solution {
    public String shortestPalindrome(String s) {
        if (s == null || s.isEmpty()) return s;

        int idx = 0;
        char[] T = new char[2 * s.length() + 3];
        T[idx++] = '^';
        T[idx++] = '#';
        for (char c : s.toCharArray()) {
            T[idx++] = c;
            T[idx++] = '#';
        }
        T[idx++] = '$';

        int[] P = new int[T.length];
        int center = 0, right = 0;
        // 回文串左侧是起始字符的最大中心
        int maxCenter = center;
        for (int i = 1; i < T.length - 1; i++) {
            if (i < right) P[i] = Math.min(right - i, P[2 * center - i]);

            while (T[i - P[i] - 1] == T[i + P[i] + 1]) P[i]++;

            if (i + P[i] > right) {
                center = i;
                right = i + P[i];

                // 新的中心回文串包含左侧起始字符，更新
                if (center - P[center] <= 1) maxCenter = center;
            } 
        }
		// 截取剩余字符串
        StringBuffer part = new StringBuffer(s.substring(P[maxCenter]));
        // 翻转拼接到原字符串前面
        part.reverse();
        part.append(s);
        return part.toString();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了95.59%的用户。

内存消耗：39.2MB，在所有java提交中击败了25.00%的用户。

## 官方解题

&emsp;官方还提供了借鉴`KMP`的思路。