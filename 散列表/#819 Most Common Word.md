[toc]

Given a paragraph and a list of banned words, return the most frequent word that is not in the list of banned words.  It is guaranteed there is at least one word that isn't banned, and that the answer is unique.

Words in the list of banned words are given in lowercase, and free of punctuation.  Words in the paragraph are not case sensitive.  The answer is in lowercase.



Note:

* $1 \le \text{paragraph.length} \le 1000$.
* $0 \le \text{banned.length} \le 100$.
* $1 \le \text{banned[i].length} \le 10$.
* The answer is unique, and written in lowercase (even if its occurrences in `paragraph` may have uppercase symbols, and even if it is a proper noun.)
* `paragraph` only consists of letters, spaces, or the punctuation symbols `!?',;`.
* There are no hyphens or hyphenated words.
* Words only consist of letters, never apostrophes or other punctuation symbols.



## 题目解读

&emsp;统计出现最多次且不在禁用名单上的词。

```java
class Solution {
    public String mostCommonWord(String paragraph, String[] banned) {

    }
}
```

## 程序设计

* 使用哈希表计数统计。

```java
class Solution {
    public String mostCommonWord(String paragraph, String[] banned) {
        if (paragraph == null || paragraph.isEmpty()) return null;

        Map<String, Integer> counter = new HashMap<>();
        // 将禁用词设置为负数
        for (String ban : banned) counter.put(ban, -1);

        char[] seqs = paragraph.toCharArray();
        StringBuffer sb = new StringBuffer();
        // 记录次数最大的字符串（次数相同则取字典序较小的）
        int maxCount = 0;
        String maxStr = null;
        for (int i = 0; i < seqs.length; i++) {
            if (Character.isLetter(seqs[i])) {
                // 将字符转化为小写字符
                sb.append(Character.toLowerCase(seqs[i]));
            } else {
                String word = sb.toString();
                // 计数（单词非空且不是禁用词）
                if (!word.isEmpty() && counter.getOrDefault(word, 0) != -1) {
                    counter.put(word, counter.getOrDefault(word, 0) + 1);
					// 更新记录
                    if (counter.get(word) > maxCount
                            || counter.get(word) == maxCount && word.compareTo(maxStr) < 0) {
                        maxCount = counter.get(word);
                        maxStr = word;
                    }
                }
                sb = new StringBuffer();
            }
        }
        // 循环结束后，处理最后的字符串
        if (sb.length() != 0) {
            String word = sb.toString();
            if (counter.getOrDefault(word, 0) != -1) {
                counter.put(word, counter.getOrDefault(word, 0) + 1);

                if (counter.get(word) > maxCount
                        || counter.get(word) == maxCount && word.compareTo(maxStr) < 0) {
                    maxCount = counter.get(word);
                    maxStr = word;
                }
            }
        }
        return maxStr;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\max(M,N))$，空间复杂度为$O(M + N)$。

执行用时：9ms，在所有java提交中击败了79.72%的用户。

内存消耗：39.3MB，在所有java提交中击败了14.29%的用户。

## 官方解题

&emsp;官方思路类似。