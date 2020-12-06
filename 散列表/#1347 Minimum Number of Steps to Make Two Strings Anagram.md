[toc]

Given two equal-size strings `s` and `t`. In one step you can choose **any character** of `t` and replace it with **another character**.

Return the minimum number of steps to make `t` an anagram of `s`.

An **Anagram** of a string is a string that contains the same characters with a different (or the same) ordering.

 

**Example 1**:

```
Input: s = "bab", t = "aba"
Output: 1
Explanation: Replace the first 'a' in t with b, t = "bba" which is anagram of s.
```

**Example 2**:

```
Input: s = "leetcode", t = "practice"
Output: 5
Explanation: Replace 'p', 'r', 'a', 'i' and 'c' from t with proper characters to make t anagram of s.
```

**Example 3**:

```
Input: s = "anagram", t = "mangaar"
Output: 0
Explanation: "anagram" and "mangaar" are anagrams. 
```

**Example 4**:

```
Input: s = "xxyyzz", t = "xxyyzz"
Output: 0
```

**Example 5**:

```
Input: s = "friend", t = "family"
Output: 4
```



**Constraints**:

* $1 \le \text{s.length} \le 50000$
* $\text{s.length} == \text{t.length}$
* `s` and `t` contain lower-case English letters only.



## 题目解读

&emsp;求最少的替换数使得`t`为`s`的字母异位词。

```java
class Solution {
    public int minSteps(String s, String t) {

    }
}
```

## 程序设计

* 分别对两个字符串统计计数，然后以目标字符串字符为准，计算需要填补的数目。

```java
class Solution {
    public int minSteps(String s, String t) {
        if (s == null || t == null || s.length() != t.length()) throw new IllegalArgumentException("invalid param");
        int[] counter1 = new int[26];
        for (char c : s.toCharArray()) counter1[c - 'a']++;
        int[] counter2 = new int[26];
        for (char c : t.toCharArray()) counter2[c - 'a']++;

        int res = 0;
        for (int i = 0; i < 26; i++) {
            if (counter1[i] > counter2[i]) res += counter1[i] - counter2[i];
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：8 ms, 在所有 Java 提交中击败了95.01%的用户

内存消耗：39.3 MB, 在所有 Java 提交中击败了38.74%的用户。

## 官方解题

&emsp;官方思路类似。