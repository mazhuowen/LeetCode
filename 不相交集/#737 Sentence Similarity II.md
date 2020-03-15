[toc]

Given two sentences `words1`, `words2` (each represented as an array of strings), and a list of similar word pairs `pairs`, determine if two sentences are similar.

For example, `words1 = ["great", "acting", "skills"]` and `words2 = ["fine", "drama", "talent"]` are similar, if the similar word pairs are `pairs = [["great", "good"], ["fine", "good"], ["acting","drama"], ["skills","talent"]]`.

Note that the similarity relation **is** transitive. For example, if "great" and "good" are similar, and "fine" and "good" are similar, then "great" and "fine" **are similar**.

Similarity is also symmetric. For example, "great" and "fine" being similar is the same as "fine" and "great" being similar.

Also, a word is always similar with itself. For example, the sentences `words1 = ["great"]`, `words2 = ["great"]`, `pairs = []` are similar, even though there are no specified similar word pairs.

Finally, sentences can only be similar if they have the same number of words. So a sentence like `words1 = ["great"]` can never be similar to `words2 = ["doubleplus","good"]`.

Note:

* The length of `words1` and `words2` will not exceed `1000`.
* The length of `pairs` will not exceed `2000`.
* The length of each `pairs[i]` will be `2`.
* The length of each `words[i]` and `pairs[i][j]` will be in the range `[1, 20]`.



## 题目解读

&emsp;给定近义词词表，判断两个句子是否近似。首先句子长度必须相等，其次同一个单词即使不在词表也是相近的。

```java
class Solution {
    public boolean areSentencesSimilarTwo(String[] words1, String[] words2, List<List<String>> pairs) {

    }
}
```

## 程序设计

* 将近义词词表表示为不相交集，然后遍历句子判断。

```java
class Solution {
    public boolean areSentencesSimilarTwo(String[] words1, String[] words2, List<List<String>> pairs) {
        if(pairs == null || pairs.size() == 0) return false;
        if(words1 == null || words2 == null || words1.length != words2.length) return false;
        // 维护单词、索引映射对
        Map<String, Integer> idxMap = new HashMap<>();
        DisJoint disJoint = new DisJoint(pairs.size() * 2);
        int idx = 0;
        for(List<String> pair : pairs) {
            if(idxMap.get(pair.get(0)) == null) idxMap.put(pair.get(0), idx++);
            if(idxMap.get(pair.get(1)) == null) idxMap.put(pair.get(1), idx++);
            // 关联
            disJoint.union(disJoint.find(idxMap.get(pair.get(0))), disJoint.find(idxMap.get(pair.get(1))));
        }
        for (int i = 0; i < words1.length; i++) {
            // 如果当前单词相等，则继续（不管是否在相近词表中存在）
            if(words1[i].equals(words2[i])) continue;
            // 查看是否在相近词表存在
            if(idxMap.get(words1[i]) == null || idxMap.get(words2[i]) == null) return false;
            // 查看是否相近
            if(disJoint.find(idxMap.get(words1[i])) != disJoint.find(idxMap.get(words2[i]))) return false;
        }
        return true;
    }
}

class DisJoint {
    private int size;
    private int[] tree;

    DisJoint(int size) {
        this.size = size;
        this.tree = new int[size];
        Arrays.fill(tree, -1);
    }

    public void union(int root1, int root2) {
        if(tree[root1] >= 0 && tree[root2] >= 0) throw new IllegalArgumentException("root must be negative");
        if(root1 == root2) return;
        if(tree[root1] <= tree[root2]) {
            tree[root1] += tree[root2];
            tree[root2] = root1;
        } else {
            tree[root2] += tree[root1];
            tree[root1] = root2;
        }
        size--;
    }

    public int find(int idx) {
        if(tree[idx] < 0) return idx;
        return tree[idx] = find(tree[idx]);
    }

    public int size() {
        return size;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(max(N,M))$，空间复杂度为$O(M)$，其中$N$为句子长度，$M$为近义词长度。

执行用时：36ms，在所有java提交中击败了66.20%的用户。

内存消耗：41.7MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;思路同上，还提供了深度遍历优先的思路。