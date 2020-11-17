[toc]

Implement a trie with `insert`, `search`, and `startsWith` methods.



**Note**:

* You may assume that all inputs are consist of lowercase letters `a-z`.
* All inputs are guaranteed to be non-empty strings.



## 题目解读

&emsp;实现简单的字典树。

```java
class Trie {

    /** Initialize your data structure here. */
    public Trie() {

    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {

    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {

    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {

    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
```

## 程序设计

* 由于题目限定了只考虑小写字符，多叉树的结构实现只需简单的设置为$26$位数组。对于插入就是遍历添加子节点的过程；对于查询就是遍历判断是否存在子节点的过程。需注意最后一个字符的标识判断。对于前缀判断则是简单的遍历匹配，无需考虑标识。

```java
class Trie {
    // 字典树
    private TrieNode root;

    public Trie() {
        this.root = new TrieNode();
    }
    
    public void insert(String word) {
        if (word == null || word.isEmpty()) return;
        TrieNode temp = root;
        int len = word.length();
        for (int i = 0; i < len; i++) {
            // 不存在该字符，加入
            if (temp.children[word.charAt(i) - 'a'] == null) {
                temp.children[word.charAt(i) - 'a'] = new TrieNode();
            }
            temp = temp.children[word.charAt(i) - 'a'];
            // 最后一个字符，标记
            if (i == len - 1) temp.isEnding = true;
        }
    }
    
    public boolean search(String word) {
        if (word == null || word.isEmpty()) return true;
        TrieNode temp = root;
        int len = word.length();
        for (int i = 0; i < len; i++) {
            if (temp.children[word.charAt(i) - 'a'] == null) return false;
            temp = temp.children[word.charAt(i) - 'a'];
            // 需判断结尾标识
            if (i == len - 1 && !temp.isEnding) return false; 
        }
        return true;
    }
    
    public boolean startsWith(String prefix) {
        if (prefix == null || prefix.isEmpty()) return true;
        TrieNode temp = root;
        int len = prefix.length();
        for (int i = 0; i < len; i++) {
            if (temp.children[prefix.charAt(i) - 'a'] == null) return false;
            temp = temp.children[prefix.charAt(i) - 'a'];
            // 无需判断结尾标识
        }
        return true;
    }
}

// 字典树结构
class TrieNode {
    // 结尾表示
    boolean isEnding;
    // 子节点
    TrieNode[] children = new TrieNode[26];
}
```

## 性能分析

&emsp;插入时间复杂度为$O(N)$，查询、前缀匹配时间复杂度为$O(N)$。

执行用时：41ms，在所有java提交中击败了95.21%的用户。

内存消耗：48.8MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。上述代码细节优化如下：

```java
class Trie {
    private TrieNode root;

    public Trie() {
        this.root = new TrieNode();
    }
    
    public void insert(String word) {
        if (word == null || word.isEmpty()) return;
        TrieNode temp = root;
        for (char c : word.toCharArray()) {
            if (temp.children[c - 'a'] == null) {
                temp.children[c - 'a'] = new TrieNode();
            }
            temp = temp.children[c - 'a'];
        }
        temp.isEnding = true;
    }
    
    public boolean search(String word) {
        if (word == null || word.isEmpty()) return true;
        TrieNode temp = root;
        for (char c : word.toCharArray()) {
            if (temp.children[c - 'a'] == null) return false;
            temp = temp.children[c - 'a'];
        }
        return temp.isEnding;
    }
    
    public boolean startsWith(String prefix) {
        if (prefix == null || prefix.isEmpty()) return true;
        TrieNode temp = root;
        for (char c : prefix.toCharArray()) {
            if (temp.children[c - 'a'] == null) return false;
            temp = temp.children[c - 'a'];
        }
        return true;
    }
}

class TrieNode {
    boolean isEnding;
    TrieNode[] children = new TrieNode[26];
}
```

执行用时：39ms，在所有java提交中击败了99.39%的用户。

内存消耗：50MB，在所有java提交中击败了100.00%的用户。