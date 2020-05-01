[toc]

Implement the `StreamChecker` class as follows:

* `StreamChecker(words)`: Constructor, init the data structure with the given words.
* `query(letter)`: returns true if and only if for some $k \ge 1$, the last $k$ characters queried (in order from oldest to newest, including this letter just queried) spell one of the words in the given list.



Note:

* $1 \le \text{words.length} \le 2000$
* $1 \le \text{words[i].length} \le 2000$
* Words will only consist of lowercase English letters.
* Queries will only consist of lowercase English letters.
* The number of queries is at most `40000`.



## 题目解读

&emsp;给定字符流，判断是否是给定字符串字典中出现过。

```java
class StreamChecker {

    public StreamChecker(String[] words) {

    }
    
    public boolean query(char letter) {

    }
}

/**
 * Your StreamChecker object will be instantiated and called as such:
 * StreamChecker obj = new StreamChecker(words);
 * boolean param_1 = obj.query(letter);
 */
```

## 程序设计

* 实质就是多模式串多次匹配，首先想到的是AC自动机。

```java
class StreamChecker {
    ACNode root;
    // 记录每个字符匹配的位置
    ACNode p;

    public StreamChecker(String[] words) {
        // 构造字典树
        this.root = new ACNode(' ');
        this.p = root;
        for (String word : words) {
            ACNode temp = root;
            for (char c : word.toCharArray()) {
                int idx = c - 'a';
                if (temp.children[idx] == null) temp.children[idx] = new ACNode(c);
                temp = temp.children[idx];
            }
            temp.isEnding = true;
            temp.length = word.length();
        }
        // 维护失败指针
        buildFailPointer();
    }

    private void buildFailPointer() {
        Queue<ACNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            ACNode p = queue.poll();
            for (int i = 0; i < 26; i++) {
                ACNode pc = p.children[i];
                if (pc == null) continue;

                if (p == root) pc.fail = root;
                else {
                    ACNode q = p.fail;
                    while (q != null && q.children[i] == null) {
                        q = q.fail;
                    }

                    if (q == null) pc.fail = root;
                    else pc.fail = q.children[i];
                }
                queue.add(pc);
            }
        }
    }
    
    public boolean query(char letter) {
        int idx = letter - 'a';
        while (this.p != root && p.children[idx] == null) {
            p = p.fail;
        }
        p = p.children[idx];
        if (p == null) p = root;
		
        // 迭代后p为当前字符位置
        ACNode temp = p;
        while (temp != root) {
            // 判断所有失败指针指向的以p结尾的字符串是否是结束标识
            if (temp.isEnding) return true;
            temp = temp.fail;
        }

        return false;
    }
}

// AC自动机结点
class ACNode {
    char c;
    boolean isEnding;
    int length = -1;

    ACNode[] children = new ACNode[26];
    ACNode fail;

    ACNode(char c) {
        this.c = c;
    }
}
```

## 性能分析

&emsp;构建时间复杂度为$O(L)$，$L$为总的字符串长度；查询时间复杂度为$O(1)$；空间复杂度为$O(L)$。

执行用时：281ms，在所有java提交中击败了32.86%的用户。

内存消耗：72.2MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区常用的思路是前缀树记录给定单词的翻转，即后缀树，然后记录截止目前的字符串，并倒序匹配。AC自动机更优雅，不需要记录之前的字符。

```java
class StreamChecker {
    Trie root;
    StringBuffer sb;

    public StreamChecker(String[] words) {
        root = new Trie(' ');
        sb = new StringBuffer();

        for (String word : words) {
            Trie temp = root;
            // 倒序构建
            char[] strs = word.toCharArray();
            for (int i = strs.length - 1; i >= 0; i--) {
                char c = strs[i];
                if (temp.children[c - 'a'] == null) temp.children[c - 'a'] = new Trie(c);
                temp = temp.children[c - 'a'];
            }
            temp.isEnding = true;
        }
    }
    
    public boolean query(char letter) {
        sb.append(letter);
        Trie temp = root;
        for (int i = sb.length() - 1; i >= 0; i--) {
            char c = sb.charAt(i);
            // 无法匹配
            if (temp.children[c - 'a'] == null) return false;
            temp = temp.children[c - 'a'];
            if (temp.isEnding) return true;
        }
        return false;
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

&emsp;构建字典树时间复杂度为$O(L)$，每次查询时间复杂度为$O(N)$。

执行用时：111ms，在所有java提交中击败了84.29%的用户。

内存消耗：73.7MB，在所有java提交中击败了100.00%的用户。