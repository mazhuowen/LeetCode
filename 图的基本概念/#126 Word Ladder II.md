[toc]


Given two words (`beginWord` and `endWord`), and a dictionary's word list, find all shortest transformation sequence(s) from `beginWord` to `endWord`, such that:

* Only one letter can be changed at a time
* Each transformed word must exist in the word list. Note that `beginWord` is **not** a transformed word.



**Note:**

- Return an empty list if there is no such transformation sequence.
- All words have the same length.
- All words contain only lowercase alphabetic characters.
- You may assume no duplicates in the word list.
- You may assume `beginWord` and `endWord` are non-empty and are not the same.



## 题目解读

&emsp;同[#127 Word Ladder](./#127 Word Ladder.md)，需找出最短路径的组合。

```java
class Solution {
    public List<List<String>> findLadders(String beginWord, String endWord, List<String> wordList) {

    }
}
```

## 程序设计

* 参考[#127 Word Ladder](./#127 Word Ladder.md)思路，使用双端广度优先搜索，当连通后需要拼接两端的路径。

```java
class Solution {
    List<List<String>> res;
    String beginWord;

    public List<List<String>> findLadders(String beginWord, String endWord, List<String> wordList) {
        this.res = new LinkedList<>();
        this.beginWord = beginWord;
        Set<String> dict = new HashSet<>(wordList);

        // 不符合要求
        if (!dict.contains(endWord)) return res;

        // 记录双端优先遍历端点
        Set<String> start = new HashSet<>();
        start.add(beginWord);
        Set<String> end = new HashSet<>();
        end.add(endWord);

        // 记录两侧路径，初始化加入两个端点（后续最新的点加入表头，意味着端点在最末端）
        List<List<String>> sPath = new LinkedList<>();
        List<List<String>> ePath = new LinkedList<>();
        List<String> s = new LinkedList<>();
        List<String> e = new LinkedList<>();
        s.add(beginWord);
        sPath.add(s);
        e.add(endWord);
        ePath.add(e);

        // 广度优先遍历
        findLadders(start, end, sPath, ePath, dict);
        return res;
    }

    private void findLadders(Set<String> start, Set<String> end, List<List<String>> sPath, List<List<String>> ePath, Set<String> dict) {
        // 从少的一端开始
        if (start.size() > end.size()) {
            findLadders(end, start, ePath, sPath, dict);
            return;
        }

        // 无法连通，返回
        if (start.isEmpty()) return;

        // 移除
        dict.removeAll(start);
        // 下一轮的路径
        Set<String> next = new HashSet<>();
        List<List<String>> nextPath = new LinkedList<>();

        // 是否找到最短路径
        boolean flag = false;

        // 试探
        for (List<String> sp : sPath) {
            // 生成字符串
            for (String t : geneLadder(sp.get(0).toCharArray())) {
                // 连通，则最短路径都在这一轮，无需再进行下次遍历
                if (end.contains(t)) {
                    flag = true;
                    // 路径前半段需翻转
                    List<String> pre = new LinkedList<>(sp);
                    Collections.reverse(pre);
                    // 拼接后半段
                    for (List<String> suf : ePath) {
                        if (!suf.get(0).equals(t)) continue;

                        List<String> curPath = new LinkedList<>(pre);
                        curPath.addAll(suf);
                        // 需注意判断翻转（由于双端遍历，顺序可能是反的）
                        if (!beginWord.equals(curPath.get(0))) Collections.reverse(curPath);
                        // 添加路径
                        res.add(curPath);
                    }
                }
                // 不连通，更新路径
                if (!flag && dict.contains(t)) {
                    next.add(t);
                    LinkedList<String> curPath = new LinkedList<>(sp);
                    curPath.addFirst(t);
                    nextPath.add(curPath);
                }
            } 
        }
        // 标识为true，表示已找到最短路径
        if (flag) return; 
        else findLadders(next, end, nextPath, ePath, dict);
    }

    private List<String> geneLadder(char[] t) {
        List<String> res = new LinkedList<>();
        for (int i = 0; i < t.length; i++) {
            char cur = t[i];
            for (char c = 'a'; c <= 'z'; c++) {
                if (c == cur) continue;
                t[i] = c;
                res.add(new String(t));
            }
            t[i] = cur;
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：31ms，在所有java提交中击败了80.93%的用户。

内存消耗：42MB，在所有java提交中击败了90.00%的用户。

## 官方解题

&emsp;暂无，密切关注。