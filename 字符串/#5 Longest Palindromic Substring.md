[toc]

Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000.



## 题目解读

&emsp;找到字符串中的最长回文子串。

```java
class Solution {
    public String longestPalindrome(String s) {

    }
}
```

## 程序设计

* 最基本的思路是暴力遍历：

```java
class Solution {
    public String longestPalindrome(String s) {
        String max = "";
        if (s == null || s.isEmpty()) return max;
        
        char[] chars = s.toCharArray();
        // 暴力匹配回文字符串起始结尾索引
        for (int i = 0; i < chars.length; i++) {
            for (int j = chars.length - 1; j >= i; j--) {
                // 以i开头的最长回文串，匹配到则比较更新最大回文串
                if (isPalindrome(chars, i, j)) {
                    if (j - i + 1 > max.length()) max = s.substring(i, j + 1);
                    // 当前最大回文串已找到，跳过后续遍历
                    break;
                }
            }
        }
        return max;
    }

    private boolean isPalindrome(char[] s, int i, int j) {
        while (i < j) {
            if (s[i++] != s[j--]) return false;
        }
        return true;
    }
}
```

* 上述暴力法需要三层循环，实际上回文字符串由中心向左右两侧扩散，可以遍历中心字符，然后向左向右遍历比较，这样只需要两个循环。

```java
class Solution {
    public String longestPalindrome(String s) {
        String max = "";
        if (s == null || s.isEmpty()) return max;

        for (int i = 0; i < s.length(); i++) {
            // 当前字符为中心次，扩展
            max = findMaxSeq(s, i - 1, i + 1, max);
            // 当前字符和其后字符为中心次，扩展
            if (i + 1 < s.length() && s.charAt(i) == s.charAt(i + 1)) max = findMaxSeq(s, i - 1, i + 2, max);
        }
        return max;
    }

    // 扩展
    private String findMaxSeq(String s, int left, int right, String max) {
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        } 
        if (right - left - 1 > max.length()) max = s.substring(left + 1, right);
        return max;
    }
}
```

> String可转化为字符数组，会加快时间性能。

## 性能分析

&emsp;暴力匹配时间复杂度为$O(N^3)$，空间复杂度为$O(N)$。

执行用时：167ms，在所有java提交中击败了25.07%的用户。

内存消耗：38MB，在所有java提交中击败了25.89%的用户。

&emsp;根据中心字符扩展的时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：37ms，在所有java提交中击败了66.58%的用户。

内存消耗：39.8MB，在所有java提交中击败了15.18%的用户。

## 官方解题

&emsp;官方还提供了曼彻斯特算法。

```java
class Solution {
    public String longestPalindrome(String s) {
        // 初始化数组，将偶数回文长度转化为奇数长度，一般化
        char[] T = new char[2 * s.length() + 3];
        int idx = 0;
        // 起始标志
        T[idx++] = '^';
        T[idx++] = '#';
        for (char c : s.toCharArray()) {
            T[idx++] = c;
            T[idx++] = '#';
        }
        // 结尾标志
        T[idx++] = '$';

        // 记录最长回文串中心
        int maxCenter = 0;
        // 记录每个字符为中心的最长回文字符串长度
        int[] P = new int[T.length];
        // 中心和该中心最长回文串右侧边界
        int center = 0, right = 0;
        for (int i = 1; i < T.length - 1; i++) {
            // i在当前最长回文串内部，i关于center的对称点为2*center-i，记为j
            // 如果i在左侧，此时j处没更新到，故值为0；如果i在右侧，此时j已更新，
            // i处的值为j处的值，注意i扩展不能超过right边界，j处可能中心扩展后长度超出i可扩展到right的长度，
            // 如果超出right，则right-i个长度是i至少可覆盖的回文串长度，故选择这两个中的最小值。
            if (i < right) P[i] = Math.min(right - i, P[2 * center - i]);

            // 对应三种情况，1、i不在right范围；2、上面i扩展到边界right，则此处继续扩展；
            // 3、i对应的位置j受左侧边界限制，如j是第一个字符，则j回文长度只能是1，而i显然可以继续扩展
            // 初次之外的情况即i在右侧，测扩展未超过right，此处不走
            while (T[i - P[i] - 1] == T[i + P[i] + 1]) {
                P[i]++;
            }
            // 更新最大回文串中心点
            if (P[maxCenter] < P[i]) maxCenter = i;

            // 当前i扩展超过right，更新中心点及边界
            if (i + P[i] > right) {
                center = i;
                right = i + P[i];
            }
        }
        // 最大回文串的起始位置，原始回文串起始索引为中心下标减去回文长度，除以2
        int start = (maxCenter - P[maxCenter]) / 2;
        // 截取
        return s.substring(start, start + P[maxCenter]);
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：4ms，在所有java提交中击败了97.06%的用户。

内存消耗：39.9MB，在所有java提交中击败了15.18%的用户。