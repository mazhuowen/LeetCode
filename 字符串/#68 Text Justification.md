[toc]

Given an array of words and a width `maxWidth`, format the text such that each line has exactly `maxWidth` characters and is fully (left and right) justified.

You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces `' '` when necessary so that each line has exactly `maxWidth` characters.

Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line do not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.

For the last line of text, it should be left justified and no **extra** space is inserted between words.



Note:

* A word is defined as a character sequence consisting of non-space characters only.
* Each word's length is guaranteed to be greater than 0 and not exceed `maxWidth`.
* The input array `words` contains at least one word.



## 题目解读

&emsp;文本左右对齐，一行要尽可能多的放置单词。

```java
class Solution {
    public List<String> fullJustify(String[] words, int maxWidth) {
        
    }
}
```

## 程序设计

* 除了最后一行及只有一个单词的行是左对齐，其它行都是左右对齐，意味着长度不够时，需要填充空格到单词之间。

```java
class Solution {
    public List<String> fullJustify(String[] words, int maxWidth) {
        List<String> res = new LinkedList<>();
        if (words == null || words.length == 0) return res;

        int idx = 0, curLen = 0;
        List<String> row = new LinkedList<>();
        while (idx < words.length) {
            // 句中加入单词及句首加入单词
            if (curLen + words[idx].length() + 1 <= maxWidth || (curLen == 0 && words[idx].length() <= maxWidth)) {
                // 除了句首，单词间加入空格
                curLen += words[idx].length() + (curLen == 0 ? 0 : 1);
                row.add(words[idx]);
            }
            // 换行
            else {
                // 填充组合单词
                res.add(formatRow(row, maxWidth, false));
                // 新的一行
                curLen = words[idx].length();
                row = new LinkedList<>();
                row.add(words[idx]);
            }
            idx++;
        }
        // 最后一行
        res.add(formatRow(row, maxWidth, true));
        return  res;
    }

    private String formatRow(List<String> sents, int maxWidth, boolean isLast) {
        StringBuffer sb = new StringBuffer();
        if (isLast) {
            // 单词间拼接空格
            for (String token : sents) sb.append(token).append(" ");
            // 补充空格
            while (sb.length() < maxWidth) sb.append(" ");
            // 规范长度
            sb.setLength(maxWidth);
        } else {
            // 统计填充空格数
            int len = maxWidth;
            int count = 0;
            for (String token : sents) {
                len -= token.length();
                count += 1;
            }

            // 剩余的len个空格需要分配到count - 1个地方
            for (String token : sents) {
                sb.append(token);
                // 最后一个单词无需拼接空格
                if (count == 1) break;
                // 计算当前空格长度
                int curLen = len / (count - 1) + (len % (count - 1) == 0 ? 0 : 1);
                len -= curLen;
                count--;
                for (int i = 0; i < curLen; i++) {
                    sb.append(" ");
                }
            }
            // 对应这一行只有一个单词的情况，必须左对齐，补全后侧
            while (sb.length() < maxWidth) sb.append(" ");
        }

        return sb.toString();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了85.54%的用户。

内存消耗：38.1MB，在所有java提交中击败了20.00%的用户。

## 官方解题

&emsp;暂无，密切关注。