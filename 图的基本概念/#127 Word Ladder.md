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

&emsp;官方提出了双向广度优先搜索。