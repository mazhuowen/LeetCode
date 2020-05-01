[toc]

Implement a magic directory with `buildDict`, and `search` methods.

For the method `buildDict`, you'll be given a list of non-repetitive words to build a dictionary.

For the method `search`, you'll be given a word, and judge whether if you modify **exactly** one character into **another** character in this word, the modified word is in the dictionary you just built.



Note:

* You may assume that all the inputs are consist of lowercase letters `a-z`.
* For contest purpose, the test data is rather small by now. You could think about highly efficient algorithm after the contest.
* Please remember to **RESET** your class variables declared in class MagicDictionary, as static/class variables are **persisted across multiple test cases**. 



## 题目解读

&emsp;判断给定的单词是否和字典中某一个字符串只有一个字符的差距。

```java
class MagicDictionary {
    
    /** Initialize your data structure here. */
    public MagicDictionary() {
        
    }
    
    /** Build a dictionary through a list of words */
    public void buildDict(String[] dict) {
        
    }
    
    /** Returns if there is any word in the trie that equals to the given word after modifying exactly one character */
    public boolean search(String word) {
        
    }
}

/**
 * Your MagicDictionary object will be instantiated and called as such:
 * MagicDictionary obj = new MagicDictionary();
 * obj.buildDict(dict);
 * boolean param_2 = obj.search(word);
 */
```

## 程序设计

* 前缀树的搜索。

```java
class MagicDictionary {
    Trie root;

    /** Initialize your data structure here. */
    public MagicDictionary() {
        this.root = new Trie(' ');
    }
    
    /** Build a dictionary through a list of words */
    public void buildDict(String[] dict) {
        // 常规构建字典树
        for (String s : dict) {
            Trie temp = root;
            for (char c : s.toCharArray()) {
                if (temp.children[c - 'a'] == null) temp.children[c - 'a'] = new Trie(c);
                temp = temp.children[c - 'a'];
            }
            temp.isEnding = true;
        }
    }
    
    /** Returns if there is any word in the trie that equals to the given word after modifying exactly one character */
    public boolean search(String word) {
        if (word == null || word.isEmpty()) return true;
        return match(word, 0, root, false);
    }

    // replace表示之前是否替换过字符，idx和cur表示当前匹配字符
    private boolean match(String s, int idx, Trie cur, boolean replace) {
        // 匹配完成且必须有一次替换，返回成功
        if (cur.isEnding && idx == s.length() && replace) return true;
        // 未匹配成功
        if (idx >= s.length()) return false;

        char c = s.charAt(idx);
        // 之前已经替换过，后续不能再替换，必须原串比较
        if (replace) {
            // 未匹配到
            if (cur.children[c - 'a'] == null) return false;
            // 继续匹配下一个字符
            return match(s, idx + 1, cur.children[c - 'a'], replace);
        }

        // 之前未替换过，尝试替换
        for (int i = 0; i < 26; i++) {
            Trie next = cur.children[i];
            if (next == null) continue;
			
            // 当前字符一致，继续比较
            if (next.c == c) {
                if (match(s, idx + 1, next, replace)) return true;
            } 
            // 当前字符不一致，尝试替换
            else {
                if (match(s, idx + 1, next, true)) return true;
            }
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

## 性能分析

&emsp;构建时间复杂度为$O(L)$，匹配时间复杂度为$O(N)$；空间复杂度为$O(L)$。

执行用时：12ms，在所有java提交中击败了89.56%的用户。

内存消耗：37.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方提供了广义邻居的思路。