[toc]

Given a 2D board and a list of words from the dictionary, find all words in the board.

Each word must be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

Note:

* All inputs are consist of lowercase letters `a-z`.
* The values of words are distinct.



## 题目解读

&emsp;给定单词板，给出能够构成给定链表中的单词。

```java
class Solution {
    public List<String> findWords(char[][] board, String[] words) {
        
    }
}
```

## 程序设计

* 实质是单词的前缀匹配。首先将单词列表生成字典树，然后遍历矩形回溯判断。

```java
class Solution {
    int[] delta = new int[]{0, 1, 0, -1, 0};
    char[][] board;
    boolean[][] visited;
    // 去重
    Set<String> set;

    public List<String> findWords(char[][] board, String[] words) {
        List<String> res = new LinkedList<>();
        if (words == null || words.length == 0) return res;

        // 构建字典树
        Trie root = new Trie();
        for (String word : words) {
            root.insert(word);
        }

        int m = board.length, n = board[0].length;
        this.board = board;
        this.visited = new boolean[m][n];
        // 去重
        this.set = new HashSet<>();
        // 回溯匹配
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // 回溯
                findWords(root, i, j, new StringBuffer());   
            }
        }
        res = new LinkedList<>(set);
        return res;
    }

    // 回溯判断
    private void findWords(Trie root, int i, int j, StringBuffer word) {
        if (i < 0 || i >= board.length || j < 0 || j >= board[0].length) return;
        char cur = board[i][j];
        // 已访问或不匹配
        if (visited[i][j] || root.children[cur - 'a'] == null) return;

        // 标记试探
        visited[i][j] = true;
        root = root.children[cur - 'a'];
        word.append(cur);
        // 如果是单词结尾字符，加入答案
        if (root.isEnding) set.add(word.toString());

        for (int k = 0; k < 4; k++) {
            // 矩形试探
            int x = i + delta[k], y = j + delta[k + 1];
            findWords(root, x, y, word);
        }
        // 回溯
        word.setLength(word.length() - 1);
        visited[i][j] = false;
    }
}

// 字典树
class Trie {
    boolean isEnding;
    Trie[] children = new Trie[26];

    public void insert(String word) {
        if (word == null || word.isEmpty()) return;

        Trie temp = this;
        for (char c : word.toCharArray()) {
            if (temp.children[c - 'a'] == null) temp.children[c - 'a'] = new Trie();
            temp = temp.children[c - 'a'];
        }
        temp.isEnding = true;
     }

    public boolean search(String word) {
         if (word == null || word.isEmpty()) return true;
         Trie temp = this;
         for (char c : word.toCharArray()) {
            if (temp.children[c - 'a'] == null) return false;
            temp = temp.children[c - 'a'];
        }
        return temp.isEnding;
    }
}
```

* 优化上述代码，在回溯完之后将该单词结尾标识删除，这样就不会存在重复问题了，可以去除排重逻辑。

```java
class Solution {
    int[] delta = new int[]{0, 1, 0, -1, 0};
    char[][] board;
    boolean[][] visited;
    List<String> res;

    public List<String> findWords(char[][] board, String[] words) {
        res = new LinkedList<>();
        if (words == null || words.length == 0) return res;

        // 构建字典树
        Trie root = new Trie();
        for (String word : words) {
            root.insert(word);
        }

        int m = board.length, n = board[0].length;
        this.board = board;
        this.visited = new boolean[m][n];

        // 回溯匹配
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // 回溯
                findWords(root, i, j, new StringBuffer());   
            }
        }
        return res;
    }

    private void findWords(Trie root, int i, int j, StringBuffer word) {
        if (i < 0 || i >= board.length || j < 0 || j >= board[0].length) return;
        char cur = board[i][j];
        // 不匹配
        if (visited[i][j] || root.children[cur - 'a'] == null) return;

        // 标记试探
        visited[i][j] = true;
        word.append(cur);
        // 如果是单词结尾，加入答案
        if (root.children[cur - 'a'].isEnding) res.add(word.toString());

        for (int k = 0; k < 4; k++) {
            // 矩形试探
            int x = i + delta[k], y = j + delta[k + 1];
            findWords(root.children[cur - 'a'], x, y, word);
        }
        // 回溯
        word.setLength(word.length() - 1);
        visited[i][j] = false;
        // 剪枝
        if (root.children[cur - 'a'].isEnding) root.children[cur - 'a'].isEnding = false;
    }
}

class Trie {
    boolean isEnding;
    Trie[] children = new Trie[26];

    public void insert(String word) {
        if (word == null || word.isEmpty()) return;

        Trie temp = this;
        for (char c : word.toCharArray()) {
            if (temp.children[c - 'a'] == null) temp.children[c - 'a'] = new Trie();
            temp = temp.children[c - 'a'];
        }
        temp.isEnding = true;
     }
}
```

## 性能分析

&emsp;构建字典树需花费$O(L)$，$L$为所有单词总长，最坏情况回溯需要花费$O(MN4^{MN})$；空间复杂度为$O(L)$存储字典树，回溯花费$O(MN)$。

执行用时：13ms，在所有java提交中击败了81.11%的用户。

内存消耗：48.1MB，在所有java提交中击败了40.00%的用户。

## 官方解题

&emsp;官方解题思路同上。