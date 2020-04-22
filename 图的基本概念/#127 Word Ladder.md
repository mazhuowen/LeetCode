[toc]

Given two words (`beginWord` and `endWord`), and a dictionary's word list, find the length of shortest transformation sequence from `beginWord` to `endWord`, such that:

* Only one letter can be changed at a time.
* Each transformed word must exist in the word list.



Note:

* Return 0 if there is no such transformation sequence.
* All words have the same length.
* All words contain only lowercase alphabetic characters.
* You may assume no duplicates in the word list.
* You may assume `beginWord` and `endWord` are non-empty and are not the same.



## 题目解读

&emsp;给出词语接龙的最短路径。

```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        
    }
}
```

## 程序设计

* 实际是有向无环图最短路径的计算，即广度优先搜索。

```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        if (beginWord == null || endWord == null || beginWord.length() != endWord.length() || beginWord.equals(endWord)) throw new IllegalArgumentException("invalid param");

        int len = 0;
        Queue<String> queue = new LinkedList<>();
        queue.add(beginWord);
        while (!queue.isEmpty()) {
            // 层数加一
            len++;
            // 同层字符串出队
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                String cur = queue.poll();
                // 到达终点
                if (cur.equals(endWord)) return len;
                // 入队并将入队的元素从链表删除
                List<String> temp = new LinkedList<>();
                for (String s : wordList) {
                    // 如果符合条件，加入队列
                    if (isLadder(cur, s)) queue.add(s);
                    else temp.add(s);
                }
                wordList = temp;
            }
        }

        return 0;
    }

    private boolean isLadder(String s1, String s2) {
        if (s1.length() != s2.length()) return false;

        int count = 0;
        for (int i = 0; i < s1.length(); i++) {
            if (s1.charAt(i) != s2.charAt(i)) count++;
        }

        return count == 1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：875ms，在所有java提交中击败了26.91%的用户。

内存消耗：40.8MB，在所有java提交中击败了33.33%的用户。

## 官方解题

&emsp;官方提出了双向广度优先搜索。借鉴社区时间性能较好的思路：

```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        if (beginWord == null || endWord == null || beginWord.length() != endWord.length() || beginWord.isEmpty() || wordList.isEmpty()) return 0;

        Set<String> dict = new HashSet<>(wordList);
        if (!dict.contains(endWord)) return 0;
        // 使用集合代替队列，进行广度优先搜索
        Set<String> start = new HashSet<>();
        Set<String> end = new HashSet<>();
        start.add(beginWord);
        end.add(endWord);

        return bfs(start, end, dict, 2, beginWord.length());
    }

    private int bfs(Set<String> start, Set<String> end, Set<String> dict, int dis, int len) {
        // 一端不存在符合要求的值，不连通，返回0
        if (start.isEmpty()) return 0;
        // 从小的一端出发
        if (start.size() > end.size()) return bfs(end, start, dict, dis, len);

        // 删除已遍历点（必须放在此处，由于是两端广度优先搜索，start端到达这些点，如果
        // 下次换从end端口遍历，删除可能会断开链接；而过上次是start端遍历这次又是start端遍历，则可删除
        // 因为即使下次是end端遍历，这些旧的start点用不到）
        dict.removeAll(start);
        // 下一层
        Set<String> next = new HashSet<>();

        for (String t : start) {
            for (String s : geneLadder(t)) {
                if (dict.contains(s)) {
                    // 连通，返回
                    if (end.contains(s)) return dis;
                    next.add(s);
                }
            }
        }

        return bfs(next, end, dict, dis + 1, len);
    }

    // 生成下一个词语的集合
    private List<String> geneLadder(String s) {
        List<String> res = new ArrayList<>();

        char[] strs = s.toCharArray();
        for (int i = 0; i < s.length(); i++) {
            char temp = strs[i];
            for (char c = 'a'; c <= 'z'; c++) {
                if (c == temp) continue;
                strs[i] = c;
                res.add(new String(strs));
            }
            strs[i] = temp;
        }
        return res;
    }
}
```

执行用时：16ms，在所有java提交中击败了95.61%的用户。

内存消耗：40.3MB，在所有java提交中击败了33.33%的用户。