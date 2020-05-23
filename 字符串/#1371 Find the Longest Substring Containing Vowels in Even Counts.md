[toc]

Given the string `s`, return the size of the longest substring containing each vowel an even number of times. That is, `'a'`, `'e'`, `'i'`, `'o'`, and `'u'` must appear an even number of times.



Constraints:

* $1 \le \text{s.length} \le 5 \times 10^5$
* `s` contains only lowercase English letters.



## 题目解读

&emsp;求给定字符串的最长子串，其中的元音字符总是偶数个。

```java
class Solution {
    public int findTheLongestSubstring(String s) {

    }
}
```

## 程序设计

* 类似前缀和，记录每个序列之前的五个元音字符数目，遍历比对。

```java
class Solution {
    public int findTheLongestSubstring(String s) {
        if (s == null || s.isEmpty()) return 0;
        char[] str = s.toCharArray();

        // 记录截止位置i元音j的数目
        int[][] record = new int[str.length + 1][5];
        for (int i = 0; i < str.length; i++) {
            int idx = isVowel(str[i]);
            if (idx != -1) record[i + 1][idx] = record[i][idx] + 1;

            for (int j = 0; j < 5; j++) {
                if (j == idx) continue;
                record[i + 1][j] = record[i][j];
            }
        }

        // 遍历判断区间i～j内元音字符数目是否符合要求
        int maxLen = 0;
        for (int i = 0; i <= str.length; i++) {
            for (int j = 0; j <= i; j++) {
                boolean flag = true;
                for (int k = 0; k < 5; k++) {
                    if ((record[i][k] - record[j][k]) % 2 == 1) {
                        flag = false;
                        break;
                    }
                }
                // 符合要求则更新距离
                if (flag) {
                    maxLen = Math.max(maxLen, i - j);
                    break;
                }
            }
        }
        return maxLen;
    }

    private int isVowel(char c) {
        switch (c) {
            case 'a':
                return 0;
            case 'e':
                return 1;
            case 'i':
                return 2;
            case 'o':
                return 3;
            case 'u':
                return 4;
            default:
                return -1;
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：552ms，在所有java提交中击败了5.14%的用户。

内存消耗：75.5MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方思路采用状态压缩巧妙解决。只需关注元音字符的奇偶性，而无需关注具体数目，可用5位的二进制数来表示五个元音的奇偶性。

```java
class Solution {
    public int findTheLongestSubstring(String s) {
        if (s == null || s.isEmpty()) return 0;
        char[] str = s.toCharArray();

        // 记录每个状态对应的第一个位置，状态元音字符为偶数个初始化为位置0（此处0类似前缀和初始化下标0）
        int[] record = new int[1 << 5];
        Arrays.fill(record, -1);
        record[0] = 0;
        int res = 0, status = 0;
        for (int i = 0; i < str.length; i++) {
            if (str[i] == 'a') {
                status ^= 1 << 0;
            } else if (str[i] == 'e') {
                status ^= 1 << 1;
            } else if (str[i] == 'i') {
                status ^= 1 << 2;
            } else if (str[i] == 'o') {
                status ^= 1 << 3;
            } else if (str[i] == 'u') {
                status ^= 1 << 4;
            }

            // 之前存在该状态
            if (record[status] != -1) {
                res = Math.max(res, i + 1 - record[status]);
            } 
            // 状态第一次出现
            else {
                record[status] = i + 1;
            }
        }
        return res;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：15ms，在所有java提交中击败了83.80%的用户。

内存消耗：44.3MB，在所有java提交中击败了100.00%的用户。