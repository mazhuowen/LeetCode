[toc]

Given a non-empty list of words, return the k most frequent elements.

Your answer should be sorted by frequency from highest to lowest. If two words have the same frequency, then the word with the lower alphabetical order comes first.

Note:

* You may assume k is always valid, $1 \le k \le \text{number}$ of unique elements.
* Input words contain only lowercase letters.

Follow up:

* Try to solve it in $O(n\log_2k)$time and$O(n)$extra space.



## 题目解读

&emsp;给定只包含小写字母的单词数组，返回频率最多的$k$个单词。题目要求按照频率排序，如果频率相同则按照字典序排序。

```java
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        
    }
}
```

## 程序设计

* 首先使用字典统计单词频率，然后入容量为$k$的最小堆。最后堆中就是前$k$个单词。但是最小堆有个问题就是拼接完成后需要翻转，才能使链表头为最大频率结点，但是翻转后字典序又会被颠倒。故选择使用最大堆。

```java
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        List<String> res = new LinkedList<>();
        Map<String, Integer> counter = new HashMap<>(words.length);
        for(String word : words) {
            counter.put(word, counter.getOrDefault(word, 0) + 1);
        }
        // 最大堆
        PriorityQueue<Map.Entry<String, Integer>> queue = new PriorityQueue<>((a, b) -> {   
            // 频率相同，比较字典序
            if(a.getValue() == b.getValue()) {
                return a.getKey().compareTo(b.getKey());
            } else {
                return b.getValue() - a.getValue();
            }
        });
        for(Map.Entry<String, Integer> entry : counter.entrySet()) {
            queue.add(entry);
        }
        while(!queue.isEmpty() && k > 0) {
            res.add(queue.poll().getKey());
            k--;
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：9ms，在所有java提交中击败了79.43%的用户。

内存消耗：46.1MB，在所有java提交中击败了5.15%的用户。

## 官方解题

&emsp;官方除了提供最简单的排序后取前$k$个，还提供了最小堆的思路。使用先入队再出队，巧妙的消除了字典序问题。

```java
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        Map<String, Integer> count = new HashMap<>();
        for (String word: words) {
            count.put(word, count.getOrDefault(word, 0) + 1);
        }
        // 最小堆
        PriorityQueue<String> heap = new PriorityQueue<String>(
                (w1, w2) -> count.get(w1).equals(count.get(w2)) ?
                w2.compareTo(w1) : count.get(w1) - count.get(w2) );

        // 关键地方，先入队再出队而不是先出队在入队，解决了字典序的问题
        for (String word: count.keySet()) {
            heap.offer(word);
            if (heap.size() > k) 
                heap.poll();
        }

        List<String> ans = new ArrayList<>();
        while (!heap.isEmpty()) 
            ans.add(heap.poll());
        Collections.reverse(ans);
        return ans;
    }
}
```

&emsp;时间复杂度为$O(N\log_2k)$，空间复杂度为$O(N)$。

执行用时：13ms，在所有java提交中击败了21.98%的用户。

内存消耗：46.7MB，在所有java提交中击败了5.15%的用户。