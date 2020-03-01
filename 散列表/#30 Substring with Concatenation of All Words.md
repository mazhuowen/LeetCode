[toc]

You are given a string, s, and a list of words, words, that are all of the same length. Find all starting indices of substring(s) in s that is a concatenation of each word in words exactly once and without any intervening characters.



## 题目解读

&emsp;给定字符串和一些长度相等的单词列表，找出字符串中恰好由这些单词组成的子串的起始位置。注意子串要与单词列表中的单词完全匹配，中间不能有其他字符，但不需要考虑 **words** 中单词串联的顺序。注意单词列表中可以存在重复单词，需要跟面试官沟通。

```java
class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        
    }
}
```

## 程序设计

* 最容易想到的是从每个字符遍历其后的子串长度，检查这其中是否有不符合要求的值。

```java
class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        List<Integer> res = new LinkedList<>();
        if(s == null || s.isEmpty() || words.length == 0) {
            return res;
        }
        // 记录words中单词和数目
        Map<String, Integer> map = new HashMap<>();
        for(String word : words) {
            map.put(word, map.getOrDefault(word, 0) + 1);
        }
        // 记录单词数目
        int num = words.length;
        // 记录单词的长度
        int len = words[0].length();
        // 队列
        Queue<String> queue = new LinkedList<>();
        // 从每个字符向后遍历
        for(int i = 0; i <= s.length() - len * num; i++) {
            // 以i为起始检查其后的len * num个元素
            int idx = i;
            while(idx + len <= i + len * num) {
                String cur = s.substring(idx, idx + len);
                // 当前单词存在与列表中且可加入
                if(map.keySet().contains(cur) && map.get(cur) > 0) {
                    // 入队，计数减一
                    queue.add(cur);
                    map.put(cur, map.get(cur) - 1);
                    // 队列中存在一组合适的解，记录
                    if(!queue.isEmpty() && queue.size() == num) {
                        res.add(i);
                        // 恢复计数
                        while(!queue.isEmpty()) {
                            map.put(queue.peek(), map.get(queue.poll()) + 1);
                        }
                        break;
                    }
                    idx += len;
                }
                // 当前单词不存在与列表中或可用数目已用完
                else {
                    // 恢复计数
                    while(!queue.isEmpty()) {
                        map.put(queue.peek(), map.get(queue.poll()) + 1);
                    }
                    break;
                }
            }
        }
        return res;
    }
}
```

* 如果一次遍历字符串，则可以想象为队列，当遇到队列中不存在或计数不超过要求的打次时入队；当遇到单词列表中不存在的单词时，清空队列；当遇到超过计数的单词时，出队直到不超出计数。
* 该方法有个问题，如字符串为`aaaaaaaa`，单词列表为`[aa, aa, aa]`，只会检查到起始位置为0、2的序列，不会检查到起始位置为1的序列，因为入队、出队操作都是以单词长度`len`为步长的，会略过上述情况。解决方法就是在外层加入偏移循环。

```java
class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        List<Integer> res = new LinkedList<>();
        if(s == null || s.isEmpty() || words.length == 0) {
            return res;
        }
        // 记录words中单词和数目
        Map<String, Integer> map = new HashMap<>();
        for(String word : words) {
            map.put(word, map.getOrDefault(word, 0) + 1);
        }
        // 记录单词数目
        int num = words.length;
        // 记录单词的长度
        int len = words[0].length();
        // 记录起始索引
        int pre;
        // 偏移（非常重要）
        for(int offset = 0; offset < len; offset++) {
            Map<String, Integer> temp = new HashMap<>();
            pre = offset;
            for(int idx = offset; idx + len <= s.length(); idx += len) {
                String cur = s.substring(idx, idx + len);
                // 计数加一
                temp.put(cur, temp.getOrDefault(cur, 0) + 1);
                // 列表不存在当前单词，重新定位pre
                if(map.getOrDefault(cur, 0) == 0) {
                    pre = idx + len;
                    temp.clear();
                } 
                // 大于限定次数（出队），更新pre
                while(temp.getOrDefault(cur, 0) > map.getOrDefault(cur, 0)) {
                    String head = s.substring(pre, pre + len);
                    temp.put(head, temp.get(head) - 1);
                    pre += len;
                }
                // 满足要求，加入链表
                if(idx + len == pre + num * len) {
                    res.add(pre);
                }
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;原始算法时间复杂为度为$O(NM)$，空间复杂度为$O(M)$，其中$N$为字符串长度，$M$为$\text{words}$长度。

执行用时：168ms，在所有java提交中击败了34.18%的用户。

内存消耗：41.7MB，在所有java提交中击败了17.38%的用户。

&emsp;优化算法时间复杂度为$O(NL)$，空间复杂度为$O(M)$，其中$N$为字符串长度，$L$为单词长度，$M$为单词列表长度。由于单词长度通常远小于单词列表长度，性能更快。

执行用时：17ms, 在所有java提交中击败了79.26%的用户。

内存消耗：42.5MB，在所有java提交中击败了13.06%的用户。

## 官方解题

&emsp;暂无，密切关注。