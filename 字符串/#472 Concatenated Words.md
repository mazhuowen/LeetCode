[toc]

Given a list of words (**without duplicates**), please write a program that returns all concatenated words in the given list of words.
A concatenated word is defined as a string that is comprised entirely of at least two shorter words in the given array.



Note:

* The number of elements of the given array will not exceed `10,000`
* The length sum of elements in the given array will not exceed `600,000`.
* All the input string will only include lower case letters.
* The returned elements order does not matter.



## 题目解读

&emsp;&emsp;判断给定的数组中，哪些词是完全由另外的词拼接构成。

```java
class Solution {
    public List<String> findAllConcatenatedWordsInADict(String[] words) {

    }
}
```

## 程序设计

* 对于组合字符串，可以看做是匹配多次单个字符串，可使用前缀树实现。遍历匹配当前字符，如果遇到结尾字符，则尝试当前位置与根结点重新匹配。

```java
class Solution {
    public List<String> findAllConcatenatedWordsInADict(String[] words) {
        List<String> res = new LinkedList<>();
        if (words == null || words.length == 0) return res;

        // 加入前缀树
        Trie root = new Trie(' ');
        for (String word : words) {
            root.insert(word);
        }

        // 匹配
        for (String word : words) {
            if (root.match(word)) res.add(word);
        }

        return res;
    }
}

class Trie {
    char c;
    boolean isEnding;

    Trie[] children = new Trie[26];

    Trie(char c) {
        this.c = c;
    }

    public void insert(String s) {
        if (s == null || s.isEmpty()) return;

        Trie temp = this;
        for (char c : s.toCharArray()) {
            if (temp.children[c - 'a'] == null) temp.children[c - 'a'] = new Trie(c);
            temp = temp.children[c - 'a'];
        }
        temp.isEnding = true;
    }

    public boolean match(String s) {
        if (s == null || s.isEmpty()) return false;

        return match(s.toCharArray(), 0) == 2;
    }

    // 匹配，如果无法匹配返回-1，如果整词匹配返回1，如果组合匹配返回2
    private int match(char[] seq, int start) {
        Trie temp = this;
        for (int i = start; i < seq.length; i++) {
            int idx = seq[i] - 'a';
            if (temp.children[idx] == null) return -1;
            temp = temp.children[idx];

            // 深度搜索，从i+1位置重新匹配，尝试是否是组合
            if (temp.isEnding && match(seq, i + 1) != -1) return 2;
        }
        return temp.isEnding ? 1 : -1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$，$N$为字符串总长度。

执行用时：53ms，在所有java提交中击败了91.95%的用户。

内存消耗：51.1MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区时间性能较好的方法使用集合处理。

```java
class Solution {
    private Set<String> wordSet;

    public List<String> findAllConcatenatedWordsInADict(String[] words) {

        wordSet = new HashSet<>();
        for (String word : words) {
            wordSet.add(word);
        }

        List<String> result = new ArrayList<String>();
        Arrays.asList(words).parallelStream().forEach(word -> {
            if (dfs(word, 0, 0, new StringBuilder())) {
                result.add(word);
            }
        });

        return result;
    }

    // start为字符起始位置，count为截止当前位置的单词数
    private boolean dfs(String word, int count, int start, StringBuilder sb) {
        for (int i = start; i < word.length(); i++) {
            sb.append(word.charAt(i));
            
            boolean isWord = wordSet.contains(sb.toString());
            // 当前为一个单词则继续遍历尝试
            if (isWord && dfs(word, count + 1, i + 1, new StringBuilder())) {
                return true;
            }
        }
        
        // 返回单词数多于1，且最后一部分是单词
        return count > 1 && (wordSet.contains(sb.toString()) || start == word.length());
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：42ms，在所有java提交中击败了99.66%的用户。

内存消耗：46.7MB，在所有java提交中击败了100.00%的用户。