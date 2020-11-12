[toc]

Given a string `s`, return the length of the longest substring between two equal characters, excluding the two characters. If there is no such substring return $-1$.

A **substring** is a contiguous sequence of characters within a string.



**Constraints:**

- $1 \le \text{s.length} \le 300$
- `s` contains only lowercase English letters.



## 题目解读

&emsp;统计字符串中相同字母之间最长长度。

```java
class Solution {
    public int maxLengthBetweenEqualCharacters(String s) {

    }
}
```

## 程序设计

* 使用数组记录字母首次出现位置，然后遍历判断。

```java
class Solution {
    public int maxLengthBetweenEqualCharacters(String s) {
        // 记录字符首次出现的索引
        int[] index = new int[26];
        Arrays.fill(index, -1);
        int max = -1;
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (index[c - 'a'] == -1) index[c - 'a'] = i;
            else max = Math.max(max, i - index[c - 'a'] - 1);
        }
        return max;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了82.33%的用户。

内存消耗：36.5MB，在所有java提交中击败了34.53%的用户。

## 官方解题

&emsp;暂无，密切关注。