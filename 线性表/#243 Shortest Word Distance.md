[toc]

Given a list of words and two words word1 and word2, return the shortest distance between these two words in the list.



Note:
You may assume that word1 **does not equal to** word2, and word1 and word2 are both in the list.



## 题目解读

&emsp;给定数组，计算指定的两个单词之间的最短距离。注意数组中单词存在重复。

```java
class Solution {
    public int shortestDistance(String[] words, String word1, String word2) {

    }
}
```

## 程序设计

* 使用两个变量记录单词索引，每次更新就计算距离。

```java
class Solution {
    public int shortestDistance(String[] words, String word1, String word2) {
        if (words == null || words.length == 0) throw new IllegalArgumentException("invalid param");

        int distance = Integer.MAX_VALUE;
        Integer idx1 = null, idx2 = null;
        for (int i = 0; i < words.length; i++) {
            if (word1.equals(words[i])) {
                idx1 = i;
                if (idx2 != null) distance = Math.min(distance, Math.abs(idx2 - idx1));
            }
            else if (word2.equals(words[i])) {
                idx2 = i;
                if (idx1 != null) distance = Math.min(distance, Math.abs(idx2 - idx1));
            }
        }
        return distance == Integer.MAX_VALUE ? -1 : distance;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了98.40%的用户。

内存消耗：40MB，在所有java提交中击败了16.67%的用户。

## 官方解题

&emsp;同上。