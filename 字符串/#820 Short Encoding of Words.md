[toc]

Given a list of words, we may encode it by writing a reference string `S` and a list of indexes `A`.

For example, if the list of words is `["time", "me", "bell"]`, we can write it as `S = "time#bell#"` and `indexes = [0, 2, 5]`.

Then for each index, we will recover the word by reading from the reference string from that index until we reach a `"#"` character.

What is the length of the shortest reference string S possible that encodes the given words?



**Note**:

* $1 \le \text{words.length} \le 2000$.
* $1 \le \text{words[i].length} \le 7$.
* Each word has only lowercase letters.



## 题目解读

&emsp;给定单词列表，求单词的最短编码长度。

```java
class Solution {
    public int minimumLengthEncoding(String[] words) {

    }
}
```

## 程序设计

* 根据示例`time`与`me`应该编码为一个，可看作字符串从尾匹配相等，则可嵌入；问题转化为字符串尾部匹配问题，自然而然想到字典树；
* 将单词尾部遍历维护入字典树后，问题就转化为树的后序遍历，只计算叶子节点的单词长度，不计算路径中的单词，需注意长度要加一，因为要拼接`#`。

```java
class Solution {
    public int minimumLengthEncoding(String[] words) {
        Trie tree = new Trie(words);
        return tree.root.dfs();
    }
}

class Trie {
    TrieNode root;

    Trie(String[] words) {
        this.root = new TrieNode(' ');
        for (String word : words) {
            TrieNode tmp = this.root;
            for (int i = word.length() - 1; i >= 0; i--) {
                char t = word.charAt(i);
                if (tmp.children[t - 'a'] == null) tmp.children[t - 'a'] = new TrieNode(t); 
                tmp = tmp.children[t - 'a'];
            }
            tmp.isEnding = true;
            tmp.length = word.length();
        }
    }

    class TrieNode {
        char c;
        boolean isEnding;
        int length;
        TrieNode[] children = new TrieNode[26];

        TrieNode(char c) {
            this.c = c;
        }

        public int dfs() {
            int res = 0;
            for (TrieNode child : this.children) {
                if (child == null) continue;
                // 非叶子节点，将叶子节点长度相加
                res += child.dfs();
            }
			
            // 叶子节点，计算长度
            if (this.isEnding && res == 0) res = this.length + 1;
            return res;
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：21ms，在所有java提交中击败了49.09%的用户。

内存消耗：46.4MB，在所有java提交中击败了5.39%的用户。

## 官方解题

&emsp;官方思路类似。社区存在将字符串逆排序，然后在插入的时候判断是否有更长的单词，若有则返回$0$，表示拼接在更长的单词中，若没有则返回单词长度，速度更快。