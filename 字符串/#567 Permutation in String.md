[toc]

Given two strings `s1` and `s2`, write a function to return true if `s2` contains the permutation of `s1`. In other words, one of the first string's permutations is the **substring** of the second string.



Note:

* The input strings only contain lower case letters.
* The length of both given strings is in range `[1, 10,000]`.



## 题目解读

&emsp;判断字符串异位结构是否在另一个字符串中出现。

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {

    }
}
```

## 程序设计

* [#438 Find All Anagrams in a String](./#438 Find All Anagrams in a String.md)的简化。

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        if (s1 == null || s1.isEmpty()) return true;
        if (s2 == null || s2.isEmpty()) return false;

        int m = s2.length(), n = s1.length();
        int[] counter = new int[26];
        for (char c : s1.toCharArray()) counter[c - 'a']++;

        int left = 0, right = 0;
        char[] str = s2.toCharArray();
        while (right < m) {
            // 字符数减少
            n--;
            counter[str[right] - 'a']--;

            // 字符不存在或字符数目不符合
            if (counter[str[right] - 'a'] < 0) {
                while (counter[str[right] - 'a'] < 0) {
                    counter[str[left++] - 'a']++;
                    n++;
                }
            }
            // 找到一组解
            else if (n == 0 && counter[str[right] - 'a'] == 0) {
                return true;
            }
            right++;
        }

        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：3ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.5MB，在所有java提交中击败了18.75%的用户。

## 官方解题

&emsp;同上。