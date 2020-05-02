[toc]

In English, we have a concept called `root`, which can be followed by some other words to form another longer word - let's call this word `successor`. For exjietiample, the root an, followed by `other`, which can form another word `another`.

Now, given a dictionary consisting of many roots and a sentence. You need to replace all the `successor` in the sentence with the `root` forming it. If a `successor` has many `roots` can form it, replace it with the root with the shortest length.

You need to output the sentence after the replacement.



Constraints:

* The input will only have lower-case letters.
* $1 \le \text{dict.length} \le 1000$
* $1 \le \text{dict[i].length} \le 100$
* $1 \le \text{sentence words number} \le 1000$
* $1 \le \text{sentence words length} \le 1000$



## 题目解读

&emsp;在英语中，词根(root)可以跟其他一些词组成另一个较长的单词——称这个词为继承词(successor)。例如词根an，跟随单词 other，可以形成新的单词 another。给定一个由许多词根组成的词典和一个句子。需要将句子中的所有继承词用词根替换掉。如果继承词有许多可以形成它的词根，则用最短的词根替换它。输出替换之后的句子。

```java
class Solution {
    public String replaceWords(List<String> dict, String sentence) {

    }
}
```

## 程序设计

* 前缀匹配，当句子中的单词匹配到第一个结束标识的字符时，将该单词替换为前缀字符串。

```java
class Solution {
    public String replaceWords(List<String> dict, String sentence) {
        if (sentence == null || sentence.isEmpty()) return sentence;
        // 构建字典树
        Trie root = new Trie(' ');
        for (String word : dict) {
            Trie temp = root;
            for (char c : word.toCharArray()) {
                if (temp.children[c - 'a'] == null) temp.children[c - 'a'] = new Trie(c);
                temp = temp.children[c - 'a'];
            }
            temp.isEnding = true;
        }
        String[] words = sentence.split(" ");
        StringBuffer sb = new StringBuffer();

        // 匹配前缀
        for (String word : words) {
            Trie temp = root;
            for (int i = 0; i < word.length(); i++) {
                char c = word.charAt(i);
                // 当前单词无前缀匹配，拼接单词
                if (temp.children[c - 'a'] == null) {
                    sb.append(word).append(" ");
                    break;
                }
                // 匹配到前缀，拼接前缀
                if (temp.children[c - 'a'].isEnding) {
                    sb.append(word.substring(0, i + 1)).append(" ");
                    break;
                }
				// 单词已匹配完，但没遇到前缀，则拼接单词
                if (i == word.length() - 1) {
                    sb.append(word).append(" ");
                    break;
                }
                // 迭代
                temp = temp.children[c - 'a'];
            }
        }
        // 删除最后空格
        sb.setLength(sb.length() - 1);
        return sb.toString();
    }
}

class Trie {
    char c;
    boolean isEnding;

    Trie[] children = new Trie[26];

    Trie(char c) {
        this.c = c;
    }
}
```

## 性能分析

&emsp;构建时间复杂度为$O(L)$，匹配时间复杂度为$O(N)$。总的时间复杂度为$O(\max(L_1, L_2))$，其中$L_1$为字典字符串总长，$L_2$为句子单词总长。空间复杂度为$O(\max(L_1,L_2))$。

执行用时：9ms，在所有java提交中击败了98.06%的用户。

内存消耗：50.5MB，在所有java提交中击败了25.00%的用户。

## 官方解题

&emsp;同上。