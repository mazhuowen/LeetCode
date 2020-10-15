[toc]

Given a list of strings, you need to find the longest uncommon subsequence among them. The longest uncommon subsequence is defined as the longest subsequence of one of these strings and this subsequence should not be **any** subsequence of the other strings.

A **subsequence** is a sequence that can be derived from one sequence by deleting some characters without changing the order of the remaining elements. Trivially, any string is a subsequence of itself and an empty string is a subsequence of any string.

The input will be a list of strings, and the output needs to be the length of the longest uncommon subsequence. If the longest uncommon subsequence doesn't exist, return $-1$.



**Note**:

* All the given strings' lengths will not exceed $10$.
* The length of the given list will be in the range of $[2, 50]$.



## 题目解读

&emsp;给定字符串数组，求最长特殊子序列。

```java
class Solution {
    public int findLUSlength(String[] strs) {

    }
}
```

## 程序设计

* 从最长字符串开始逐个比对，根据定义不能是其他字符串子序列。

```java
class Solution {
    public int findLUSlength(String[] strs) {
        Arrays.sort(strs, (a, b) -> b.length() - a.length());
        for (int i = 0; i < strs.length; i++) {
            boolean flag = true;
            for (int j = 0; j < strs.length; j++) {
                if (i == j) continue;
                // 字符串i是j的子串，不符合要求
                if (isSub(strs[i], strs[j])) flag = false;
            }

            if (flag) return strs[i].length();
        }
        return -1;
    }

    private boolean isSub(String target, String source) {
        int idx = 0;
        for (char c : source.toCharArray()) {
            if (idx < target.length() && c == target.charAt(idx)) idx++;
        }
        return idx == target.length();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2M)$，空间复杂度为$O(1)$，其中$M$为字符串最大长度。

执行用时：2ms，在所有java提交中击败了50.21%的用户。

内存消耗：35.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方思路同上。