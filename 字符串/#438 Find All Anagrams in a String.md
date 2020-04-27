[toc]

Given a string `s` and a **non-empty** string `p`, find all the start indices of `p`'s anagrams in `s`.

Strings consists of lowercase English letters only and the length of both strings `s` and `p` will not be larger than `20100`.

The order of output does not matter.



## 题目解读

&emsp;寻找异位词在字符串中的索引。

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {

    }
}
```

## 程序设计

* 队列计数，进队出队。

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> res = new LinkedList<>();

        if (s == null || s.isEmpty() || p == null || p.isEmpty()
                || s.length() < p.length()) return res;

        int m = s.length();
        // 记录p中的字符种数
        int size = 0;
        // 统计字符数
        int[] counter = new int[26];
        for (char c : p.toCharArray()) {
            if (counter[c - 'a']++ == 0) size++;
        }

        char[] str = s.toCharArray();
        // 定位窗口
        int left = 0, right = 0;
        while (right < m) {
            // 当前窗口包含多余的字符，不是异位词
            if (--counter[str[right] - 'a'] < 0) {
                // 窗口重新定位到str[right]计数为0的位置
                while (left < m && counter[str[right] - 'a'] < 0) {
                    // 计数增加，如果原先是0，则字符种数增加（对于p中不存在的字符，原先计数必然小于0）
                    if (counter[str[left++] - 'a']++ == 0) size++;
                }
            }
            // 找到一个异位词
            else if (counter[str[right] - 'a'] == 0 && --size == 0) {
                res.add(left);
                // 窗口后移一位，字符种数记为1（即出队的字符）
                counter[str[left++] - 'a']++;
                size = 1;
            }
            // 迭代
            right++;
        }

        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：5ms，在所有java提交中击败了96.96%的用户。

内存消耗：40.8MB，在所有java提交中击败了5.88%的用户。

## 官方解题

&emsp;暂无，密切关注。