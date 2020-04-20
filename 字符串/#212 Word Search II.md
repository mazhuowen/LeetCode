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

&emsp;构建字典树需花费$O(L)$，$L$为所有单词总长，最坏情况回溯需要花费$O(MN4^{L})$；空间复杂度为$O(L)$存储字典树，回溯花费$O(MN)$。

执行用时：13ms，在所有java提交中击败了81.11%的用户。

内存消耗：48.1MB，在所有java提交中击败了40.00%的用户。

## 官方解题

&emsp;官方解题思路同上。官方还提到了匹配完成后对字典树剪枝，减少后续匹配次数。分析可得只有匹配单词结尾字符是叶节点，则可删除路径上只有一个子结点的路径。如`e`、`eat`和`eeg`构成的字典树，匹配完`eat`，发现`t`为叶节点，可删除；`a`只有一个子节点且已被删除，可删除，`e`子节点除了`a`还有`e`，不能删除；如果匹配`e`，`e`有子结点，无法删除。虽然有剪枝，但是排除重复的逻辑还是要有，如前面匹配到多个`e`，则会重复，因为剪枝只会删除连通叶节点且路径单一的链路。

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

    // 返回剪枝标识
    private boolean findWords(Trie root, int i, int j, StringBuffer word) {
        if (i < 0 || i >= board.length || j < 0 || j >= board[0].length) return false;
        char cur = board[i][j];
        // 不匹配
        if (visited[i][j] || root.children[cur - 'a'] == null) return false;

        // 标记试探
        visited[i][j] = true;
        word.append(cur);
        // 如果是单词结尾，加入答案
        if (root.children[cur - 'a'].isEnding) {
            res.add(word.toString());
            // 剪枝，如果该字符是字典树叶子结点，已匹配完，可删除
            if (root.children[cur - 'a'].size == 0) {
                root.children[cur - 'a'] = null;
                // 回溯
                word.setLength(word.length() - 1);
                visited[i][j] = false;
                return --root.size == 0;
            }
        }

        for (int k = 0; k < 4; k++) {
            // 矩形试探
            int x = i + delta[k], y = j + delta[k + 1];
            // cur的字节点已匹配成功且删除，判断cur是否需要删除
            if (findWords(root.children[cur - 'a'], x, y, word)) {
                // 剪枝，只有cur一个字节点，删除cur
                if (root.children[cur - 'a'].size == 0) {
                    root.children[cur - 'a'] = null;
                    // 回溯
                    word.setLength(word.length() - 1);
                    visited[i][j] = false;
                    return --root.size == 0;
                }
            }
        }
        // 回溯
        word.setLength(word.length() - 1);
        visited[i][j] = false;

        // 排除重复（不能删除）
        if (root.children[cur - 'a'].isEnding) root.children[cur - 'a'].isEnding = false;
        return false;
    }
}

class Trie {
    // 字节点个数（用于减枝判断）
    int size;
    boolean isEnding;
    Trie[] children = new Trie[26];

    public void insert(String word) {
        if (word == null || word.isEmpty()) return;

        Trie temp = this;
        for (char c : word.toCharArray()) {
            if (temp.children[c - 'a'] == null) {
                temp.children[c - 'a'] = new Trie();
                temp.size++;
            }
            temp = temp.children[c - 'a'];
        }
        temp.isEnding = true;
    }
}
```

执行用时：11ms，在所有java提交中击败了90.43%的用户。

内存消耗：48.3MB，在所有java提交中击败了40.00%的用户。