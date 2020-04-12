[toc]

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string "".

**Note:**

All given inputs are in lowercase letters `a-z`.



## 题目解读

&emsp;给定一组字符串，给出其公共最长前缀，不存在则返回空串。

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {

    }
}
```

## 程序设计

* 设计思路为字符串与最长公共前缀两两比较得到新的前缀字符串并进行下一轮比较。

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs == null || strs.length == 0) return "";
        if (strs.length == 1) return strs[0];

        // 将数组中字符串两两比较
        String res = strs[0];
        for (int i = 1; i < strs.length; i++) {
            res = longestCommonPrefix(res, strs[i]);
            // 是空串则提前终止
            if (res.isEmpty())  break;
        } 
        return res;
    }

    // 字符串两两比较返回公共前缀
    private String longestCommonPrefix(String s, String t) {
        char[] s1 = s.toCharArray();
        char[] s2 = t.toCharArray();

        int idx = 0;
        while (idx < s1.length && idx < s2.length && s1[idx] == s2[idx]) {
            idx++;
        }
        return new String(s1, 0, idx);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(M)$，其中$N$为字符串长度总和，$M$为平均字符串长度。

执行用时：1ms，在所有java提交中击败了80.56%的用户。

内存消耗：37.8MB，在所有java提交中击败了27.02%的用户。

## 官方解题

&emsp;除了上述基本思路，还提供了分治、二分等方法。