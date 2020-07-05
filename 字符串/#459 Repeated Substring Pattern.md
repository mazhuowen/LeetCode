[toc]

Given a non-empty string check if it can be constructed by taking a substring of it and appending multiple copies of the substring together. You may assume the given string consists of lowercase English letters only and its length will not exceed 10000.



## 题目解读

&emsp;检查是否由重复子字符串构成。

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {

    }
}
```

## 程序设计

* 最基本的思路是暴力匹配。

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        if (s == null || s.length() < 2) return false;

        char[] strs = s.toCharArray();
        // 重复子字符串的数目
       	for (int num = 2; num <= strs.length; num++) {
            if (strs.length % num != 0) continue;
            int len = strs.length / num;
            boolean flag = true;
            // 暴力匹配各字符串
            point: for (int i = 0; i < len; i++) {
                char c = strs[i];
                for (int j = len; j < strs.length; j += len) {
                    if (strs[i + j] != c) {
                        flag = false;
                        break point;
                    }
                }
            }
            if (flag) return true;
        }
        return false;
    }
}
```

* 实际上可借鉴KMP算法的`next`数组，对于重复子串来说最长前缀匹配与整个字符串之间的部分就是最小重复单元。

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        int[] next = new int[s.length()];
        initNext(next, s.toCharArray());
        int len = next.length - 1 - next[next.length - 1];
        // 判断最小重复单元
        return next[next.length - 1] > -1 && next.length % len == 0;
    }

    private void initNext(int[] next, char[] s) {
        int k = -1;
        next[0] = k;
        for (int i = 1; i < s.length; i++) {
            while (k != -1 && s[i] != s[k + 1]) {
                k = next[k];
            }

            if (s[i] == s[k + 1]) k++;
            next[i] = k;
        }
    }
}
```

## 性能分析

&emsp;暴力匹配时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：5ms，在所有java提交中击败了97.73%的用户。

内存消耗：40MB，在所有java提交中击败了14.29%的用户。

&emsp;KMP时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：5ms，在所有java提交中击败了97.73%的用户。

内存消耗：39.9MB，在所有java提交中击败了14.29%的用户。

## 官方解题

&emsp;暂无，密切关注。社区还有一种思路是拼接字符串然后遍历查找，形式简洁但时间复杂度任然是$O(N^2)$。

```java
class Solution {
   public boolean repeatedSubstringPattern(String s) {
        return (s + s).indexOf(s, 1) != s.length();
    }
}
```
