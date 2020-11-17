[toc]

Design a search autocomplete system for a search engine. Users may input a sentence (at least one word and end with a special character `'#'`). For **each character** they type **except '#'**, you need to return the **top 3** historical hot sentences that have prefix the same as the part of sentence already typed. Here are the specific rules:

* The hot degree for a sentence is defined as the number of times a user typed the exactly same sentence before.
* The returned top 3 hot sentences should be sorted by hot degree (The first is the hottest one). If several sentences have the same degree of hot, you need to use ASCII-code order (smaller one appears first).
* If less than 3 hot sentences exist, then just return as many as you can.
* When the input is a special character, it means the sentence ends, and in this case, you need to return an empty list.

Your job is to implement the following functions:

The constructor function:

`AutocompleteSystem(String[] sentences, int[] times)`: This is the constructor. The input is **historical data**. `Sentences` is a string array consists of previously typed sentences. `Times` is the corresponding times a sentence has been typed. Your system should record these historical data.

Now, the user wants to input a new sentence. The following function will provide the next character the user types:

`List<String> input(char c)`: The input `c` is the next character typed by the user. The character will only be lower-case letters (`'a'` to `'z'`), blank space (`' '`) or a special character (`'#'`). Also, the previously typed sentence should be recorded in your system. The output will be the **top 3** historical hot sentences that have prefix the same as the part of sentence already typed.



**Note**:

* The input sentence will always start with a letter and end with `'#'`, and only one blank space will exist between two words.
* The number of **complete sentences** that to be searched won't exceed 100. The length of each sentence including those in the historical data won't exceed $100$.
* Please use double-quote instead of single-quote when you write test cases even for a character input.
* Please remember to **RESET** your class variables declared in class AutocompleteSystem, as static/class variables are **persisted across multiple test cases**. Please see `here` for more details.



## 题目解读

&emsp;设计补全功能，返回热度最大的前三个。

```java
class AutocompleteSystem {

    public AutocompleteSystem(String[] sentences, int[] times) {

    }
    
    public List<String> input(char c) {

    }
}

/**
 * Your AutocompleteSystem object will be instantiated and called as such:
 * AutocompleteSystem obj = new AutocompleteSystem(sentences, times);
 * List<String> param_1 = obj.input(c);
 */
```

## 程序设计

* 首先需要单词字典树补全当前用户输入单词；然后根据这个单词在句子字典树中查询匹配。
* 需注意题目中没有说明，但在示例中提到每次查询完成后需要记录更新这次查询结果；其次上面的思路有一点瑕疵，即根据补全单词再补全句子，当遇到输入为空格时返回的结果会包含之前的句子，需排除，如有句子`cc cc`和`cc cc cc`，当之前输入为`cc cc`，本次输入为空格时，还会匹配到`cc cc`，需要将`cc cc`排除，只保留`cc cc cc`。

```java
class AutocompleteSystem {
    // 字典树
    private final CharTrie wordRoot;
    private final WordTrie sentRoot;

    /* 用户前一时刻输入的字典树结点和累积字符串 */
    private CharTrie preChar;
    // 保存当前字符单词
    private StringBuffer preChars = new StringBuffer();
    private WordTrie preWord;
    // 保存当前单词句子
    private StringBuffer preSents = new StringBuffer();

    public AutocompleteSystem(String[] sentences, int[] times) {
        // 构建字典树
        this.wordRoot = new CharTrie();
        this.sentRoot = new WordTrie();
        this.preChar = wordRoot;
        this.preWord = sentRoot;

        for (int i = 0; i < sentences.length; i++) {
            String sent = sentences[i];
            int count = times[i];
            // 切分
            String[] words = sent.split(" ");

            sentRoot.insert(words, count);

            for (String word : words) {
                // 将单词中的特殊符号去除
                word = word.replace("#", "");
                if (word.isEmpty()) continue;

                wordRoot.insert(word, count);
            }
        }
    }

    public List<String> input(char c) {
        List<String> res = new LinkedList<>();
        // 句子结束，保存历史记录，清空前一状态，返回空
        if (c == '#') {
            // 保存历史记录
            if (preChars.length() > 0) {
                if (preSents.length() > 0) preSents.append(" ");
                preSents.append(preChars);
                saveHistory(preSents.toString());
            }
            // 清空
            preChar = wordRoot;
            preChars = new StringBuffer();
            preWord = sentRoot;
            preSents = new StringBuffer();
            return res;
        }

        List<Pair> tempRes = new LinkedList<>();
        // 表示单词结束，拼接入句子
        if (c == ' ') {
            // 更新句子匹配器
            if (preWord != null) preWord = preWord.children.get(preChars.toString());
            // 将单词更新入句子
            if (preSents.length() == 0) preSents.append(preChars);
            else preSents.append(" ").append(preChars);
            // 重置单词匹配器
            preChar = wordRoot;
            preChars = new StringBuffer();

            // 更新当前句子匹配器
            if (preWord != null) preWord.search(preSents, tempRes);
            // 删除空格前的句子
            if (tempRes.size() > 0 && tempRes.get(0).getKey().equals(preSents.toString())) tempRes.remove(0);
        }
        // 表示当前单词
        else {
            // 更新单词匹配器
            if (preChar != null) preChar = preChar.children[c - 'a'];
            preChars.append(c);

            // 匹配单词
            List<String> words = new LinkedList<>();
            if (preChar != null) preChar.search(preChars, words);

            // 当前输入匹配不到单词，返回空集合
            if (words.isEmpty() || preWord == null) return res;

            for (String word : words) {
                WordTrie curWord = preWord.children.get(word);
                StringBuffer curSents = new StringBuffer(preSents);
                if (curSents.length() != 0) curSents.append(" ");
                curSents.append(word);
                if (curWord != null) {
                    curWord.search(curSents, tempRes);
                }
            }
        }

        // 处理合并
        tempRes.sort((a, b) -> b.getValue() == a.getValue() ? a.getKey().compareTo(b.getKey()) : b.getValue() - a.getValue());

        for (int i = 0; i < 3 && i < tempRes.size(); i++) {
            res.add(tempRes.get(i).getKey());
        }

        return res;
    }

    private void saveHistory(String s) {
        String[] words = s.split(" ");

        sentRoot.insert(words, 1);

        for (String word : words) {
            // 将单词中的特殊符号去除
            word = word.replace("#", "");
            if (word.isEmpty()) continue;

            wordRoot.insert(word, 1);
        }
    }
}

// 单词字典树
class CharTrie {
    // 结尾表示
    boolean isEnding;
    // 子节点
    CharTrie[] children = new CharTrie[26];

    public void insert(String word, int count) {
        CharTrie temp = this;
        for (char c : word.toCharArray()) {
            if (temp.children[c - 'a'] == null) temp.children[c - 'a'] = new CharTrie();
            // 迭代
            temp = temp.children[c - 'a'];
        }
        // 更新尾标识
        temp.isEnding = true;
    }

    // 匹配当前字符之后的所有单词（prePart为当前字符之前的字符串）
    public void search(StringBuffer prePart, List<String> res) {
        // 匹配到单词
        if (isEnding) res.add(prePart.toString());

        int len = prePart.length();
        for (int i = 0; i < 26; i++) {
            CharTrie cur = children[i];
            if (cur == null) continue;

            // 尝试
            prePart.append((char)(i + 'a'));
            cur.search(prePart, res);

            // 回溯
            prePart.setLength(len);
        }
    }
}

// 单词字典树
class WordTrie {
    // 结尾表示
    boolean isEnding;
    // 热度
    int degree;
    // 子节点
    Map<String, WordTrie> children = new HashMap<>();

    public void insert(String[] sentence, int count) {
        WordTrie temp = this;
        for (String word : sentence) {
            // 将单词中的特殊符号去除
            word = word.replace("#", "");
            if (word.isEmpty()) continue;

            if (temp.children.get(word) == null) temp.children.put(word, new WordTrie());

            // 迭代
            temp = temp.children.get(word);
        }
        temp.degree += count;
        temp.isEnding = true;
    }

    // 匹配当前字符之后的所有单词（prePart为当前字符之前的字符串）
    public void search(StringBuffer prePart, List<Pair> res) {
        // 匹配到句子
        if (isEnding) res.add(new Pair(prePart.toString(), degree));

        int len = prePart.length();

        for (Map.Entry<String, WordTrie> entry : children.entrySet()) {
            WordTrie cur = entry.getValue();
            String w = entry.getKey();
            // 尝试
            prePart.append(" ").append(w);
            cur.search(prePart, res);

            // 回溯
            prePart.setLength(len);
        }
    }
}

class Pair {
    String key;
    int value;

    public Pair(String key, int value) {
        this.key = key;
        this.value = value;
    }

    public String getKey() {
        return key;
    }

    public int getValue() {
        return value;
    }
}
```

## 性能分析

执行用时：251ms，在所有java提交中击败了43.48%的用户。

内存消耗：48.7MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方提供了哈希表和字典树的思路。事实上上述思路太繁杂，可以将句子也看做单词，大大简化流程。基础实现如下：

```java
class AutocompleteSystem {
    // 字典树
    private final Trie root;
    // 记录前一结点
    private Trie pre;
    // 记录历史字符串
    private StringBuffer sb;

    public AutocompleteSystem(String[] sentences, int[] times) {
        this.root = new Trie();
        this.pre = root;
        sb = new StringBuffer();

        for (int i = 0; i < sentences.length; i++) {
            root.insert(sentences[i], times[i]);
        }
    }
    
    public List<String> input(char c) {
        List<String> res = new LinkedList<>();
        // 终止
        if (c == '#') {
            // 更新历史记录
            saveHistory(sb.toString());
            // 清空状态
            pre = root;
            sb = new StringBuffer();
            return res;
        }
        
        // 记录历史记录
        sb.append(c);
        List<Pair<String, Integer>> temp = new ArrayList<>();

        // 更新结点
        if (pre != null) pre = c == ' ' ? pre.children[26] : pre.children[c - 'a'];
        // 搜索其后所有值
        if (pre != null) pre.search(sb, temp);

        // 排序
        temp.sort((a, b) -> a.getValue() == b.getValue() ? a.getKey().compareTo(b.getKey()) : b.getValue() - a.getValue());

        for (int i = 0; i < 3 && i < temp.size(); i++) {
            res.add(temp.get(i).getKey());
        }

        return res;
    }

    private void saveHistory(String s) {
        root.insert(s, 1);
    }
}

class Trie {
    boolean isEnding;
    int count;
    // 最后一位存放空格
    Trie[] children = new Trie[27];

    public void insert(String word, int val) {
        if (word == null || word.isEmpty()) return;

        Trie temp = this;
        for (char c : word.toCharArray()) {
            int idx = c == ' ' ? 26 : (c - 'a');
            if (temp.children[idx] == null) temp.children[idx] = new Trie();

            temp = temp.children[idx];
        }
        // 结尾字符标记及计数更新
        temp.isEnding = true;
        temp.count += val;
    }

    public void search(StringBuffer prePart, List<Pair<String, Integer>> res) {
        // 匹配到一个句子，加入
        if (isEnding) res.add(new Pair<>(prePart.toString(), count));

        int len = prePart.length();
        for (int i = 0; i < 27; i++) {
            Trie child = children[i];
            if (child == null) continue;

            char c = i == 26 ? ' ' : (char)(i + 'a');
            // 试探
            prePart.append(c);
            child.search(prePart, res);
            // 回溯
            prePart.setLength(len);
        }
    }
}
```

&emsp;构建时间复杂度为$O(L)$，$L$为总长；匹配时间复杂度最坏为$O(L)$。

执行用时：260ms，在所有java提交中击败了41.62%的用户。

内存消耗：49.3MB，在所有java提交中击败了100.00%的用户。

&emsp;上述代码可继续优化，每次遍历所有结果并排序太耗时，可以在字典树内部构建数据结构维护关系，查询直接取出。

```java
class AutocompleteSystem {
    // 字典树
    private final Trie root;
    // 记录前一结点
    private Trie pre;
    // 记录历史字符串
    private StringBuffer sb;

    public AutocompleteSystem(String[] sentences, int[] times) {
        this.root = new Trie(3);
        this.pre = root;
        sb = new StringBuffer();

        for (int i = 0; i < sentences.length; i++) {
            root.insert(sentences[i], times[i]);
        }
    }

    public List<String> input(char c) {
        List<String> res = new LinkedList<>();
        // 终止
        if (c == '#') {
            // 更新历史记录
            saveHistory(sb.toString());
            // 清空状态
            pre = root;
            sb = new StringBuffer();
            return res;
        }

        // 记录历史记录
        sb.append(c);

        // 更新结点
        if (pre != null) pre = c == ' ' ? pre.children[26] : pre.children[c - 'a'];
        // 搜索其后所有值
        if (pre != null) pre.search(res);

        return res;
    }

    private void saveHistory(String s) {
        root.insert(s, 1);
    }
}

class Trie {
    // 只有结尾结点才有的属性
    boolean isEnding;
    int count;
    String s;

    Trie[] children;
    // 最小堆保存最大的k个
    int k;
    PriorityQueue<Trie> queue;

    Trie(int k) {
        this.k = k;
        // 最小堆
        this.queue = new PriorityQueue<>((a, b) -> a.count == b.count ? b.s.compareTo(a.s) : a.count - b.count);
        // 最后一位存放空格
        this.children = new Trie[27];
    }

    public void insert(String word, int val) {
        if (word == null || word.isEmpty()) return;

        Trie temp = this;
        // 记录路径上的结点
        List<Trie> path = new LinkedList<>();
        for (char c : word.toCharArray()) {
            int idx = c == ' ' ? 26 : (c - 'a');
            if (temp.children[idx] == null) temp.children[idx] = new Trie(k);

            temp = temp.children[idx];
            path.add(temp);
        }
        // 结尾字符标记及计数更新
        temp.isEnding = true;
        temp.count += val;
        temp.s = word;

        // 关联终止节点到路径上每个结点
        for (Trie cur : path) {
            // 移除老的值
            if (cur.queue.contains(temp)) cur.queue.remove(temp);
            // 加入新的值
            cur.queue.add(temp);
            // 大于k个，将最小的弹出
            while (cur.queue.size() > k) cur.queue.poll();
        }
    }

    public void search(List<String> res) {
        List<Trie> temp = new ArrayList<>();
        // 加入堆中元素
        while (!queue.isEmpty()) {
            Trie trie = queue.poll();
            temp.add(trie);
            res.add(trie.s);
        }
        queue.addAll(temp);
        Collections.reverse(res);
    }
}
```

执行用时：146ms，在所有java提交中击败了99.38%的用户。

内存消耗：49MB，在所有java提交中击败了100.00%的用户。