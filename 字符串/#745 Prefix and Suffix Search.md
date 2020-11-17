[toc]

Given many `words`, `words[i]` has weight `i`.

Design a class `WordFilter` that supports one function, `WordFilter.f(String prefix, String suffix)`. It will return the word with given `prefix` and `suffix` with maximum weight. If no word exists, return -1.



**Note**:

* `words` has length in range `[1, 15000]`.
* For each test case, up to `words.length` queries `WordFilter.f` may be made.
* `words[i]` has length in range `[1, 10]`.
* `prefix`, `suffix` have lengths in range `[0, 10]`.
* `words[i]` and `prefix`, `suffix` queries consist of lowercase letters only.



## 题目解读

&emsp;设计数据结构使得可以根据前缀和后缀获取权重最大的字符串。权重是字符串所在数组索引。

```java
class WordFilter {

    public WordFilter(String[] words) {

    }
    
    public int f(String prefix, String suffix) {

    }
}

/**
 * Your WordFilter object will be instantiated and called as such:
 * WordFilter obj = new WordFilter(words);
 * int param_1 = obj.f(prefix,suffix);
 */
```

## 程序设计

* 最基础的做法是构建一棵前缀树，然后根据前缀匹配所有可能的字符串，根据权重排序，然后从大到小依次判断是否包含后缀。
* 针对前缀或后缀存在空的情况做了优化，前缀存在空就查找后缀树；后缀为空就查找前缀树。

```java
class WordFilter {
    // 前后缀树
    Trie preTree;
    Trie sufTree;
    String[] words;

    public WordFilter(String[] words) {
        this.preTree = new Trie(words, true);
        // 构建后缀树
        this.sufTree = new Trie(words, false);
        this.words = words;
    }
    
    public int f(String prefix, String suffix) {
        // 前后缀为空，直接返回最大权重
        if (prefix.isEmpty() && suffix.isEmpty()) return words.length - 1;

        List<Integer> idx;
        // 后缀为空，查找前缀树
        if (suffix.isEmpty()) {
            idx = preTree.match(prefix);
            if (idx.isEmpty()) return -1;
            // 排序返回最大权重
            Collections.sort(idx);
            return idx.get(idx.size() - 1);
        } 
        // 查找后缀树
        else {
            idx = sufTree.match(new StringBuffer(suffix).reverse().toString());
            if (idx.isEmpty()) return -1;
            // 排序从大到小匹配
            Collections.sort(idx);
            for (int i = idx.size() - 1; i >= 0; i--) {
                int weight = idx.get(i);
                String word = words[weight];
                if (word.startsWith(prefix)) return weight;
            }
        }
        return -1;
    }
}

class Trie {
    TrieNode root;

    Trie(String[] words, boolean prefix) {
        this.root = new TrieNode(' ');
        for (int i = 0; i < words.length; i++) {
            String word = words[i];
            if (!prefix) word = new StringBuffer(word).reverse().toString();
            TrieNode temp = root;
            for (char c : word.toCharArray()) {
                if (temp.children[c - 'a'] == null) temp.children[c - 'a'] = new TrieNode(c);
                temp = temp.children[c - 'a'];
            }
            temp.isEnding = true;
            temp.weight = i;
        }
    }

    // 返回前缀匹配字符串列表
    public List<Integer> match(String prefix) {
        List<Integer> res = new ArrayList<>();
        TrieNode temp = root;
        for (char c : prefix.toCharArray()) {
            // 匹配不到
            if (temp.children[c - 'a'] == null) return res;
            temp = temp.children[c - 'a'];
        }
        // 匹配后面所有单词
        match(temp, res);
        return res;
    }

    private void match(TrieNode temp, List<Integer> res) {
        if (temp.isEnding) res.add(temp.weight);
        for (TrieNode next : temp.children) {
            if (next == null) continue;
            match(next, res);
        }
    }
}

class TrieNode {
    char c;
    boolean isEnding;
    int weight = -1;

    TrieNode[] children = new TrieNode[26];

    TrieNode(char c) {
        this.c = c;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(L)$，空间复杂度为$O(L)$。

执行用时：388ms，在所有java提交中击败了22.36%的用户。

内存消耗：60.5MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方解题通过改变字符串结构，巧妙的利用一个前缀树完成。对于`apple`这个单词，可以在单词查找树插入每个后缀，后跟`#`和单词。例如，将在单词查找树中插入`#apple`, `e#apple`, `le#apple`, `ple#apple`, `pple#apple`, `apple#apple`。然后对于`prefix = "ap"`, `suffix = "le"`这样的查询，可以通过查询单词查找树找到`le#ap`。

```java
class WordFilter {
    Trie tree;
    String[] words;

    public WordFilter(String[] words) {
        this.tree = new Trie(words);
        this.words = words;
    }
    
    public int f(String prefix, String suffix) {
        return tree.getWeight(suffix + "#" + prefix);
    }
}

class Trie {
    TrieNode root;

    Trie(String[] words) {
        this.root = new TrieNode(' ');
        for (int i = 0; i < words.length; i++) {
            String word = words[i];
            for (int j = 0; j <= word.length(); j++) {
                TrieNode temp = root;
                // 后缀拼接前缀
                String s = word.substring(j) + "#" + word;
                for (char c : s.toCharArray()) {
                    int idx = c == '#' ? 26 : (c - 'a');
                    if (temp.children[idx] == null) temp.children[idx] = new TrieNode(c);
                    temp = temp.children[idx];
                    // 更新路径的最大权重（按照顺序，每次权重大于之前）
                    temp.weight = i;
                }
                temp.isEnding = true;
            }
        }
    }

    public int getWeight(String s) {
        TrieNode temp = root;
        for (char c : s.toCharArray()) {
            int idx = c == '#' ? 26 : (c - 'a');
            if (temp.children[idx] == null) return -1;
            temp = temp.children[idx];
        }
        // 返回权重即可
        return temp.weight;
    }
}

class TrieNode {
    char c;
    boolean isEnding;
    int weight = -1;

    TrieNode[] children = new TrieNode[27];

    TrieNode(char c) {
        this.c = c;
    }
}
```

&emsp;时间复杂度为$O(N)$，$N$为前缀后缀字符串长度之和。空间复杂度为$O(L)$。

执行用时：217ms，在所有java提交中击败了91.30%的用户。

内存消耗：81.8MB，在所有java提交中击败了100.00%的用户。