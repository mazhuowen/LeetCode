[toc]

Given a set of keywords `words` and a string `S`, make all appearances of all keywords in `S` bold. Any letters between `<b>` and `</b>` tags become bold.

The returned string should use the least number of tags possible, and of course the tags should form a valid combination.

For example, given that `words = ["ab", "bc"]` and `S = "aabcd"`, we should return `"a<b>abc</b>d"`. Note that returning `"a<b>a<b>b</b>c</b>d"` would use more tags, so it is incorrect.



**Constraints**:

* `words` has length in range `[0, 50]`.
* `words[i]` has length in range `[1, 10]`.
* `S` has length in range `[0, 500]`.
* All characters in `words[i]` and `S` are lowercase letters.



## 题目解读

&emsp;加粗字体。

```java
class Solution {
    public String boldWords(String[] words, String S) {

    }
}
```

## 程序设计

* 难点在于字符匹配叠加的情况导致区间合并。借鉴社区时间性能较好思路，首先遍历得到当前字符为起点的最长结尾，拼接起始标志，然后继续遍历并更新结束标识，达到区间扩展的目的，直到当前位置超出结束位置，拼接结束标识。

```java
class Solution {

    private static String open = "<b>";
    private static String close = "</b>";

    public String boldWords(String[] words, String S) {
        Trie root = new Trie(words);

        char[] strs = S.toCharArray();
        StringBuilder sb = new StringBuilder();
        int maxEnd = -1;
        boolean isOpen = false;
        for (int i = 0; i < strs.length; i++) {
            Trie temp = root;
            // 以i为单词起始遍历匹配最大字符串
            int j = i;
            while (j < strs.length && temp.children[strs[j] - 'a'] != null) {
                temp = temp.children[strs[j] - 'a'];
                j++;
                if (temp.isEnding) {
                    if (!isOpen) {
                        sb.append(open);
                        isOpen = true;
                    }   
                    maxEnd = Math.max(maxEnd, j);
                }
            }
            // 当前位置超出结束位置，拼接
            if (i == maxEnd) {
                sb.append(close);
                isOpen = false;
            }
            sb.append(strs[i]);
        }

        if (isOpen) sb.append(close);

        return sb.toString();
    }
}

class Trie {
    boolean isEnding;
    Trie[] children = new Trie[26];

    Trie() {

    }

    Trie(String[] words) {
        for (String word : words) {
            Trie temp = this;
            for (char c : word.toCharArray()) {
                if (temp.children[c - 'a'] == null) temp.children[c - 'a'] = new Trie();
                temp = temp.children[c - 'a'];
            }
            temp.isEnding = true;
        }
    }
}
```

## 性能分析

&emsp;字典树时间复杂度为$O(N^2)$，空间复杂度为$O(M)$，$N$为匹配字符串长度，$M$为词表长度。

执行用时：5ms，在所有java提交中击败了98.21%的用户。

内存消耗：40.1MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。也可使用AC自动机实现，一次遍历扫描并标记区间，然后再遍历生成字符串。