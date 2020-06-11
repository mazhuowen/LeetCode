[toc]

Given a list of **unique** words, find all pairs of **distinct** indices `(i, j)` in the given list, so that the concatenation of the two words, i.e. `words[i] + words[j]` is a palindrome.



## 题目解读

&emsp;给定字符串数组，找到字符串对，使得组合为回文。

```java
class Solution {
    public List<List<Integer>> palindromePairs(String[] words) {

    }
}
```

## 程序设计

* 最基本的，暴力匹配，每次遍历一个单词，尝试将所有单词放在其后，判断是否是回文；不尝试放在其前是因为其他字符串会尝试将当前单词放在其后，即当前单词前面，这样不会造成重复。

```java
class Solution {
    public List<List<Integer>> palindromePairs(String[] words) {
        List<List<Integer>> res = new LinkedList<>();
        if (words == null || words.length == 0) return res;
        for (int i = 0; i < words.length; i++) {
            String cur = words[i];
            for (int j = 0; j < words.length; j++) {
                if (i == j) continue;
                // 尝试将j放在i后的字符串
                if (isPalindrome(words[i], words[j])) {
                    List<Integer> temp = new LinkedList<>();
                    temp.add(i);
                    temp.add(j);
                    res.add(temp);
                }
            }
        }
        return res;
    }

    private boolean isPalindrome(String prefix, String suffix) {
        int left = 0, right = suffix.length() - 1;
        // 判断两个字符串的公共部分
        while (left < prefix.length() && right >= 0) {
            if (prefix.charAt(left++) != suffix.charAt(right--)) return false;
        }

        // 其中一个字符串遍历完成，判断另一个字符串
        if (left < prefix.length()) {
            right = prefix.length() - 1;
            while (left < right) {
                if (prefix.charAt(left++) != prefix.charAt(right--)) return false;
            }
        }
        else if (right >= 0) {
            left = 0;
            while (left < right) {
                if (suffix.charAt(left++) != suffix.charAt(right--)) return false;
            }
        }
        return true;
    }
}
```

* 对于两个可以组成回文串的字符串`A`、`B`，假设`A`是较短的一个字符串，则`B`必然由`C + reverse(A)`组成，`C`必然是回文串。这样可以将所有字符串翻转并加入字典树，然后再次遍历数组匹配，此时存在两种情况：
  * 匹配串已匹配完，对应`A`较短的情况，此时需要深度遍历可能的`B`，并判断`C`是否是回文；
  * 匹配串未匹配完，在字典树中遇到`isEnding`的字符，对应`B`较短的情况，此时需要判断`A`中未匹配的部分，即`C`是否是回文。
* 考虑到特殊情况，即存在字符串为空，则记录所有的回文字符串，将该空字符串拼接到前后即可。

```java
class Solution {
    public List<List<Integer>> palindromePairs(String[] words) {
        List<List<Integer>> res = new LinkedList<>();
        if (words == null || words.length == 0) return res;

        // 保存空字符串及本来就是回文的集合
        Integer emptyIdx = null;
        Set<Integer> palindrome = new HashSet<>();

        Trie root = new Trie('/');
        for (int i = 0; i < words.length; i++) {
            String cur = words[i];
            // 记录空字符串
            if (cur == null || cur.isEmpty()) {
                emptyIdx = i;
                continue;
            }
            // 记录回文串
            if (isPalindrome(cur.toCharArray(), 0, cur.length() - 1)) {
                palindrome.add(i);
            }

            // 倒序插入
            root.insert(cur, i);
        }

        for (int i = 0; i < words.length; i++) {
            String cur = words[i];
            if (cur == null || cur.isEmpty()) continue;
            Set<Integer> idx = root.matchPalindrome(cur);
            for (int j : idx) {
                if (i == j) continue;
                List<Integer> temp = new LinkedList<>();
                temp.add(i);
                temp.add(j);
                res.add(temp);
            }
        }

        // 拼接空字符串与回文的组合
        if (emptyIdx != null) {
            for (Integer j : palindrome) {
                if (j.equals(emptyIdx)) continue;
                List<Integer> temp1 = new LinkedList<>();
                temp1.add(emptyIdx);
                temp1.add(j);
                res.add(temp1);
                List<Integer> temp2 = new LinkedList<>();
                temp2.add(j);
                temp2.add(emptyIdx);
                res.add(temp2);
            }
        }

        return res;
    }

    private boolean isPalindrome(char[] str, int start, int end) {
        while (start < end) {
            if (str[start++] != str[end--]) return false;
        }

        return true;
    }
}

class Trie {
    char c;
    // 结束字符及对应字符串编号
    boolean isEnding;
    int idx;
    String word;

    Trie[] children = new Trie[26];

    Trie(char c) {
        this.c = c;
    }

    public void insert(String s, int idx) {
        Trie temp = this;
        // 倒序插入
        for (int i = s.length() - 1; i >= 0; i--) {
            char c = s.charAt(i);
            if (temp.children[c - 'a'] == null) temp.children[c - 'a'] = new Trie(c);
            temp = temp.children[c - 'a'];
        }
        temp.isEnding = true;
        temp.idx = idx;
        temp.word = s;
    }

    public Set<Integer> matchPalindrome(String s) {
        Set<Integer> res = new HashSet<>();
        matchPalindrome(s.toCharArray(), res);
        return res;
    }

    public void matchPalindrome(char[] str, Set<Integer> res) {
        Trie temp = this;
        int i = 0;
        for (; i < str.length; i++) {
            char c = str[i];
            if (temp.children[c - 'a'] == null) return;
            temp = temp.children[c - 'a'];
            // B较短
            if (temp.isEnding && isPalindrome(str, i + 1, str.length - 1)) {
                res.add(temp.idx);
            }
        }
        // A较短
        dfs(temp, res, str.length);
    }

    private void dfs(Trie cur, Set<Integer> res, int len) {
        for (Trie child : cur.children) {
            if (child == null) continue;

            if (child.isEnding) {
                char[] str = child.word.toCharArray();
                if (isPalindrome(str, 0, str.length - len - 1)) res.add(child.idx);
            }
            dfs(child, res, len);
        }
    }

    private boolean isPalindrome(char[] str, int start, int end) {
        while (start < end) {
            if (str[start++] != str[end--]) return false;
        }

        return true;
    }
}
```

* 继续优化代码，上述存在两个缺陷：每次深度优先搜索判断是否是回文串；处理空字符串独立于字典树。针对第一个问题，可以在插入字典树的时候判断当前结点后剩余的字符串是否是回文，并维护在链表里，这样原先的深度优先搜索只需读取当前结点链表即可；对于第二个问题，由于插入时会判断是否是回文，可将是回文的字符串维护在根结点的链表里，这样遇到空字符串直接取根节点链表组合即可。

```java
class Solution {
    public List<List<Integer>> palindromePairs(String[] words) {
        List<List<Integer>> res = new LinkedList<>();
        if (words == null || words.length == 0) return res;

        // 创建字典树
        Trie trie = new Trie(words);

        // 匹配
        for (int i = 0; i < words.length; i++) {
            trie.matchPalindrome(words[i], i, res);
        }

        return res;
    }
}

class Trie {
    private TrieNode root;

    Trie(String[] words) {
        this.root = new TrieNode('/');

        for (int i = 0; i < words.length; i++) {
            String word = words[i];
            if (word == null || word.isEmpty()) continue;

            // 插入
            char[] strs = word.toCharArray();
            int[] P = palindrome(strs);
            insert(strs, P, i);
        }
    }

    private void insert(char[] strs, int[] P, int idx) {
        TrieNode temp = root;
        // 倒序插入
        for (int i = strs.length - 1; i >= 0; i--) {
            // 剩余是回文串，添加到链表
            int start = 2, end = 2 * (i + 1);
            if (P[(start + end) / 2] == i + 1) temp.palindromeIdx.add(idx);

            // 字典树插入
            char c = strs[i];
            if (temp.children[c - 'a'] == null) temp.children[c - 'a'] = new TrieNode(c);
            temp = temp.children[c - 'a'];
        }
        temp.isEnding = true;
        temp.idx = idx;
    }

    public void matchPalindrome(String word, int idx, List<List<Integer>> res) {
        // 空字符串和回文字符串前后组合
        if (word == null || word.isEmpty()) {
            for (int j : root.palindromeIdx) {
                List<Integer> list = new ArrayList<>();
                list.add(idx);
                list.add(j);
                res.add(list);
                list = new ArrayList<>();
                list.add(j);
                list.add(idx);
                res.add(list);
            }
            return;
        }

        // 非空字符串匹配
        char[] strs = word.toCharArray();
        int[] P = palindrome(strs);
        TrieNode temp = root;
        for (int i = 0; i < strs.length; i++) {
            char c = strs[i];
            if (temp.children[c - 'a'] == null) return;
            temp = temp.children[c - 'a'];

            // B较短（注意索引不能重复）
            if (temp.isEnding && temp.idx != idx) {
                // C为A的i+1~len-1子串
                int start = 2 * (i + 2), end = 2 * strs.length;
                // A剩余C为回文，AB构成回文串，添加
                if (P[(start + end) / 2] == strs.length - i - 1) {
                    List<Integer> list = new ArrayList<>();
                    list.add(idx);
                    list.add(temp.idx);
                    res.add(list);
                }
            }
        }
        // A较短，剩余B为回文，构成字符串
        for (int j : temp.palindromeIdx) {
            if (idx == j) continue;
            List<Integer> list = new ArrayList<>();
            list.add(idx);
            list.add(j);
            res.add(list);
        }
    }

    // 曼彻斯特算法
    private int[] palindrome(char[] strs) {
        char[] T = new char[2 * strs.length + 3];
        int idx = 0;
        T[idx++] = '^';
        T[idx++] = '#';
        for (char c : strs) {
            T[idx++] = c;
            T[idx++] = '#';
        }
        T[idx++] = '#';

        int[] P = new int[T.length];
        int center = 0, right = 0;
        for (int i = 1; i < T.length - 1; i++) {
            if (i < right) P[i] = Math.min(right - i, P[2 * center - i]);
            while (T[i - P[i] - 1] == T[i + P[i] + 1]) P[i]++;

            if (i + P[i] > right) {
                center = i;
                right = i + P[i];
            }
        }
        return P;
    }

    class TrieNode {
        char c;
        // 结束字符及对应字符串编号
        boolean isEnding;
        int idx;
        // 后续为回文的idx集合（节省dfs时间）
        List<Integer> palindromeIdx = new ArrayList<>();

        TrieNode[] children = new TrieNode[26];

        TrieNode(char c) {
            this.c = c;
        }
    }
}
```

## 性能分析

&emsp;暴力比较时间复杂度为$O(N^2M)$，空间复杂度为$O(N^2)$，其中$M$为平均字符串长度。

执行用时：646ms，在所有java提交中击败了8.96%的用户。

内存消耗：41.2MB，在所有java提交中击败了100.00%的用户。

&emsp;字典树时间复杂度为$O(M^2N)$，空间复杂度为$O(MN)$。

执行用时：105ms，在所有java提交中击败了30.14%的用户。

内存消耗：48MB，在所有java提交中击败了33.33%的用户。

&emsp;优化后时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：59ms，在所有java提交中击败了73.97%的用户。

内存消耗：48.9MB，在所有java提交中击败了33.33%的用户。

## 官方解题

&emsp;官方还提供了哈希表记录回文的思路，即采用两个哈希表记录前缀回文子串、后缀回文子串。

```java
class Solution {

    private List<String> allValidPrefixes(String word) {
        List<String> validPrefixes = new ArrayList<>();
        for (int i = 0; i < word.length(); i++) {
            if (isPalindromeBetween(word, i, word.length() - 1)) {
                validPrefixes.add(word.substring(0, i));
            }
        }
        return validPrefixes;
    }

    private List<String> allValidSuffixes(String word) {
        List<String> validSuffixes = new ArrayList<>();
        for (int i = 0; i < word.length(); i++) {
            if (isPalindromeBetween(word, 0, i)) {
                validSuffixes.add(word.substring(i + 1, word.length()));
            }
        }
        return validSuffixes;
    }

    // Is the prefix ending at i a palindrome?
    private boolean isPalindromeBetween(String word, int front, int back) {
        while (front < back) {
            if (word.charAt(front) != word.charAt(back)) return false;
            front++;
            back--;
        }
        return true;
    }

    public List<List<Integer>> palindromePairs(String[] words) {
        // Build a word -> original index mapping for efficient lookup.
        Map<String, Integer> wordSet = new HashMap<>();
        for (int i = 0; i < words.length; i++) {
            wordSet.put(words[i], i);
        }

        // Make a list to put all the palindrome pairs we find in.
        List<List<Integer>> solution = new ArrayList<>();

        for (String word : wordSet.keySet()) {

            int currentWordIndex = wordSet.get(word);
            String reversedWord = new StringBuilder(word).reverse().toString();

            // Build solutions of case #1. This word will be word 1.
            if (wordSet.containsKey(reversedWord)
              && wordSet.get(reversedWord) != currentWordIndex) {
                solution.add(Arrays.asList(currentWordIndex, wordSet.get(reversedWord)));
            }

            // Build solutions of case #2. This word will be word 2.
            for (String suffix : allValidSuffixes(word)) {
                String reversedSuffix = new StringBuilder(suffix).reverse().toString();
                if (wordSet.containsKey(reversedSuffix)) {
                    solution.add(Arrays.asList(wordSet.get(reversedSuffix), currentWordIndex));
                }
            }

            // Build solutions of case #3. This word will be word 1.
            for (String prefix : allValidPrefixes(word)) {
                String reversedPrefix = new StringBuilder(prefix).reverse().toString();
                if (wordSet.containsKey(reversedPrefix)) {
                    solution.add(Arrays.asList(currentWordIndex, wordSet.get(reversedPrefix)));
                }
            }
        }
        return solution;
    }
}
```

