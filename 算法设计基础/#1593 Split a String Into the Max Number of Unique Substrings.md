[toc]

Given a string `s`, return the maximum number of unique substrings that the given string can be split into.

You can split string `s` into any list of **non-empty substrings**, where the concatenation of the substrings forms the original string. However, you must split the substrings such that all of them are **unique**.

A **substring** is a contiguous sequence of characters within a string.



**Constraints:**

- $1 \le \text{s.length} \le 16$
- `s` contains only lower case English letters.



## 题目解读

&emsp;求拆分的唯一子字符串最大数目。

```java
class Solution {
    public int maxUniqueSplit(String s) {

    }
}
```

## 程序设计

* 由于字符串较短，采用回溯统计。

```java
class Solution {
    int max = 0;
    
    public int maxUniqueSplit(String s) {
        backTracing(s.toCharArray(), 0, new HashSet<>(), 0);
        return max;
    }
    
    private void backTracing(char[] seq, int idx, Set<String> set, int count) {
        if (idx >= seq.length) {
            max = Math.max(max, count);
            return;
        }
        
        for (int i = idx; i < seq.length; i++) {
            // 尝试切分方案
            String sub = new String(seq, idx, i + 1 - idx);
            // 已存在
            if (set.contains(sub)) continue;
            set.add(sub);
            backTracing(seq, i + 1, set, count + 1);
            // 回溯
            set.remove(sub);
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N!)$，空间复杂度为$O(N)$。

执行用时：43ms，在所有java提交中击败了77.57%的用户。

内存消耗：39.2MB，在所有java提交中击败了37.16%的用户。

## 官方解题

&emsp;暂无，密切关注。