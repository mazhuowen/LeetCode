[toc]

Given a string `s` and a list of strings **dict**, you need to add a closed pair of bold tag `<b>` and `</b>` to wrap the substrings in s that exist in dict. If two such substrings overlap, you need to wrap them together by only one pair of closed bold tag. Also, if two substrings wrapped by bold tags are consecutive, you need to combine them.



**Constraints**:

* The given dict won't contain duplicates, and its length won't exceed 100.
* All the strings in input have length in range $[1, 1000]$.



## 题目解读

&emsp;加粗字体。

```java
class Solution {
    public String addBoldTag(String s, String[] dict) {
        
    }
}
```

## 程序设计

* 最基本的思路是Trie树检测标记加粗子字符串，标记后再做处理。

```java
class Solution {
    static final String OPEN = "<b>";
    static final String CLOSE = "</b>";

    public String addBoldTag(String s, String[] dict) {
        if (dict == null || dict.length == 0 || s == null || s.length() == 0) return s;

        Trie trie = new Trie(dict);
        StringBuffer sb = new StringBuffer();
        // 加粗起始标记
        boolean isOpen = false;
        int maxEnd = -1;
        for (int i = 0; i < s.length(); i++) {
            // 以i为开头匹配单词
            int end = trie.match(s, i);
            maxEnd = Math.max(maxEnd, end);
            // 加入起始标记
            if (!isOpen && end != -1) {
                isOpen = true;
                sb.append(OPEN);
            }
            // 当前未匹配到，加入结束标识
            if (maxEnd != -1 && maxEnd == i - 1) {
                sb.append(CLOSE);
                isOpen = false;
            }
            sb.append(s.charAt(i));
        }
        if (isOpen) sb.append(CLOSE);
        return sb.toString();
    }
}

class Trie {
    TrieNode root;

    Trie(String[] words) {
        this.root = new TrieNode(' ');
        for (String word : words) {
            TrieNode temp = this.root;
            for (char c : word.toCharArray()) {
                if (temp.children.get(c) == null) temp.children.put(c, new TrieNode(c));
                temp = temp.children.get(c);
            }
            temp.isEnding = true;
        }
    }

    // 匹配最长匹配串
    public int match(String s, int idx) {
        int res = -1;
         TrieNode temp = this.root;
         for (int i = idx; i < s.length(); i++) {
             char c = s.charAt(i);
             if (temp.children.get(c) != null) temp = temp.children.get(c);
             else break;
             if (temp.isEnding) res = i;
         }
         return res;
    }


    class TrieNode {
        char c;
        boolean isEnding;
        Map<Character, TrieNode> children = new HashMap<>();

        TrieNode(char c) {
            this.c = c;
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(N)$。

执行用时：56ms，在所有java提交中击败了29.41%的用户。

内存消耗：55.3MB，在所有java提交中击败了10.53%的用户。

## 官方解题

&emsp;除了上述思路，官方采用循环遍历标记然后处理的思路，实现更简洁。

```java
class Solution {
    public String addBoldTag(String s, String[] dict) {
        // 标记
        boolean mark[] = new boolean[s.length()];
        for(String word: dict) {
            int fromIndex = 0;
            while(fromIndex < s.length()) {
                // 匹配
                int idx = s.indexOf(word, fromIndex);
                if(idx < 0) break;
                
                // 标记
                for(int j = idx; j < idx + word.length(); j++) mark[j] = true;
                // 迭代
                fromIndex = idx + 1;
            }
        }
        // 处理标记
        StringBuilder ans = new StringBuilder();
        for (int i = 0; i < s.length(); ++i) {
            if (mark[i] && (i == 0 || !mark[i-1])) ans.append("<b>");
            ans.append(s.charAt(i));
            if (mark[i] && (i == s.length()-1 || !mark[i+1])) ans.append("</b>");
        }
        return ans.toString();
    }
}
```

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(N)$。

执行用时：6ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.5MB，在所有java提交中击败了94.74%的用户。