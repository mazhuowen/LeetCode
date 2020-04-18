[toc]

Design a data structure that supports the following two operations:

```java
void addWord(word)
bool search(word)
```

`search(word)` can search a literal word or a regular expression string containing only letters `a-z` or `.`. A `.` means it can represent any one letter.



**Note:**
You may assume that all words are consist of lowercase letters `a-z`.



## 题目解读

&emsp;只考虑小写字母的字典树匹配。

```java
class WordDictionary {

    /** Initialize your data structure here. */
    public WordDictionary() {

    }
    
    /** Adds a word into the data structure. */
    public void addWord(String word) {

    }
    
    /** Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter. */
    public boolean search(String word) {

    }
}

/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary obj = new WordDictionary();
 * obj.addWord(word);
 * boolean param_2 = obj.search(word);
 */
```

## 程序设计

* 增加了通配符，实际上就是树的遍历搜索相结合。

```java
class WordDictionary {
    private TrieNode root;

    public WordDictionary() {
        this.root = new TrieNode();
    }
    
    public void addWord(String word) {
        if (word == null || word.isEmpty()) return;
        TrieNode temp = root;
        for (char c : word.toCharArray()) {
            // 添加
            if (temp.children[c - 'a'] == null) {
                temp.children[c - 'a'] = new TrieNode();
            }
            temp = temp.children[c - 'a'];
        }
        // 结尾字符设置标识
        temp.isEnding = true;
    }
    
    public boolean search(String word) {
        if (word == null || word.isEmpty()) return true;
        // 前序遍历
        return search(0, word.toCharArray(), root);
    }

    private boolean search(int idx, char[] word, TrieNode parent) {
        char c = word[idx];
        boolean last = idx == word.length - 1;
        // 如果是通配符，则前序遍历
        if (c == '.') {
            for (int i = 0; i < 26; i++) {
                if (parent.children[i] == null) continue;
                // 如果是结尾字符，则判断标识
                if (last) {
                    if (parent.children[i].isEnding) return true;
                }
                // 前序遍历
                else if (search(idx + 1, word, parent.children[i])) return true;
            }
            // 所有子节点都不符合要求
            return false;
        } else {
            // 不存在字符
            if (parent.children[c - 'a'] == null) return false;
            else if (last) return parent.children[c - 'a'].isEnding;
            return search(idx + 1, word, parent.children[c - 'a']);
        }
    }
}

class TrieNode {
    boolean isEnding;
    TrieNode[] children = new TrieNode[26];
}
```

## 性能分析

&emsp;字符串插入时间复杂度为$O(N)$，查询时间复杂度最坏情况全是通配符，时间复杂度为$O(M)$，$M$为树中所有字符数。空间复杂度为$O(M)$。

执行用时：49ms，在所有java提交中击败了90.71%的用户。

内存消耗：50.4MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区还有使用哈希表的思路，较为基础不再阐述。上述代码结构可优化为：

```java
class WordDictionary {
    private TrieNode root;

    public WordDictionary() {
        this.root = new TrieNode();
    }
    
    public void addWord(String word) {
        if (word == null || word.isEmpty()) return;
        TrieNode temp = root;
        for (char c : word.toCharArray()) {
            // 添加
            if (temp.children[c - 'a'] == null) {
                temp.children[c - 'a'] = new TrieNode();
            }
            temp = temp.children[c - 'a'];
        }
        // 结尾字符设置标识
        temp.isEnding = true;
    }
    
    public boolean search(String word) {
        if (word == null || word.isEmpty()) return true;
        // 前序遍历
        return search(0, word.toCharArray(), root);
    }

    private boolean search(int idx, char[] word, TrieNode parent) {
        if (parent == null) return false;
        char c = word[idx];
        // 终止条件
        if (idx == word.length - 1) {
            if (c != '.') return parent.children[c - 'a'] != null && parent.children[c - 'a'].isEnding;
            // 通配符则遍历子节点找到结束字符
            for (TrieNode child : parent.children) {
                if (child != null && child.isEnding) return true;
            }
            // 未找到
            return false;
        }
        
        // 不是通配符，返回
        if (c != '.')  return parent.children[c - 'a'] != null && search(idx + 1, word, parent.children[c - 'a']);

        // 如果是通配符，则前序遍历
        for (TrieNode child : parent.children) {
            // 前序遍历
            if (search(idx + 1, word, child)) return true;
        }
        // 所有子节点都不符合要求
        return false;
    }
}

class TrieNode {
    boolean isEnding;
    TrieNode[] children = new TrieNode[26];
}
```

执行用时：49ms，在所有java提交中击败了90.71%的用户。

内存消耗：50.4MB，在所有java提交中击败了100.00%的用户。