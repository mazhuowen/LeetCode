[toc]

Given strings `S` and `T`, find the minimum (contiguous) **substring** `W` of `S`, so that `T` is a **subsequence** of `W`.

If there is no such window in `S` that covers all characters in `T`, return the empty string `"".` If there are multiple such minimum-length windows, return the one with the left-most starting index.



**Note**:

* All the strings in the input will only contain lowercase letters.
* The length of `S` will be in the range `[1, 20000]`.
* The length of `T` will be in the range `[1, 100]`.



## 题目解读

&emsp;给定字符串`S`和`T`，找出`S`中最短的（连续）**子串**`W` ，使得`T`是`W`的**子序列** 。

```java
class Solution {
    public String minWindow(String S, String T) {

    }
}
```

## 程序设计

* 首先当从左向右匹配得到的子序列，以`bde`为例子，得到的子序列可能是`bbbdbdde`，除了最右边的字符，其它字符可能都存在重复，不是最短序列；此时可以再次从右往左遍历，这样得到的子序列是`bdde`，属于当前窗口的最短序列；以此类推，遍历整个字符串。
* 当检测到序列后，不能简单的从序列后继续遍历字符串，因为当前序列中可能存在之后序列起始的值，可以定的是当前序列是起始点对应的最短序列，故可以遍历起始点之后的点。

```java
class Solution {
    public String minWindow(String S, String T) {
        if (S == null || S.isEmpty() || T == null || T.isEmpty()) return null;

        char[] s = S.toCharArray(), t = T.toCharArray();
        int start = 0;
        String res = "";
        while (start < s.length) {
            // 正向匹配
            int end = getSubString(s, t, start);
            if (end == -1) return res;
            // 反向匹配缩减范围
            start = getCurMinString(s, t, end - 1);
            // 更新
            if (res.isEmpty() || res.length() > end - start) res = S.substring(start, end);
            // 遍历起始点之后的点
            start++;
        }
        return res;
    }

    // 返回子序列结尾后的索引
    private int getSubString(char[] s, char[] t, int start) {
        // 定位最前边的起始位置
        while (start < s.length && s[start] != t[0]) start++;
        // 不存在子序列
        if (start == s.length) return -1;
        // 定位结尾
        int end = start, idx = 0;
        while (end < s.length && idx < t.length) {
            if (s[end++] == t[idx]) idx++; 
        }
        // 不存在子序列
        if (idx < t.length) return -1;
        return end;
    }

    // 返回子序列起始的索引
    private int getCurMinString(char[] s, char[] t, int end) {
        int idx = t.length - 1;
        while (end >= 0 && idx >= 0) {
            if (s[end--] == t[idx]) idx--; 
        }
        return end + 1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：5ms，在所有java提交中击败了92.93%的用户。

内存消耗：39.8MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方提供了动态规划的思路。