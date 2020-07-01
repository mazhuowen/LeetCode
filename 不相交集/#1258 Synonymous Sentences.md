[toc]

Given a list of pairs of equivalent words `synonyms` and a sentence `text`, Return all possible synonymous sentences **sorted lexicographically**.




Constraints:

* $0 \le \text{synonyms.length} \le 10$
* $\text{synonyms[i].length} == 2$
* $\text{synonyms[i][0]} \ne \text{synonyms[i][1]}$
* All words consist of at most `10` English letters only.
* `text` is a single space separated sentence of at most `10` words.



## 题目解读

&emsp;给定近义词，替换并返回所有的组合。

```java
class Solution {
    public List<String> generateSentences(List<List<String>> synonyms, String text) {

    }
}
```

## 程序设计

* 首先注意到同义词是两两给出，需要关联为完整的同义词集合，可以选择不相交集，同时为了方便回溯按照字典序，可在不相交集中加入哈希表结构存储每个不相交集的元素。

```java
class Solution {
    public List<String> generateSentences(List<List<String>> synonyms, String text) {
        List<String> res = new LinkedList<>();
        if (text == null || text.isEmpty()) return res;

        String[] seqs = text.split(" ");
        DisJoint disJoint = new DisJoint(synonyms);
        generateSentences(seqs, 0, disJoint, res);
        return res;
    }

    public void generateSentences(String[] seqs, int idx, DisJoint disJoint, List<String> res) {
        // 跳过非同义词
        while (idx < seqs.length && disJoint.getSynonyms(seqs[idx]) == null ) idx++;
        // 答案拼接
        if (idx >= seqs.length) {
            StringBuffer sb = new StringBuffer();
            for (String seq : seqs) sb.append(seq).append(" ");
            sb.setLength(sb.length() - 1);
            res.add(sb.toString());
            return;
        }

        // 字典序试探同义词
        for (String word : disJoint.getSynonyms(seqs[idx])) {
            seqs[idx] = word;
            generateSentences(seqs, idx + 1, disJoint, res);
        }
    }
}

class DisJoint {
    int[] parent;
    // 索引映射
    Map<String, Integer> idxmap;
    // 记录不相交集元素
    Map<Integer, TreeSet<String>> rootRecord;

    DisJoint(List<List<String>> synonyms) {
        // 编号
        int size = 0;
        this.idxmap = new HashMap<>();
        for (List<String> synonym : synonyms) {
            for (String word : synonym) {
                if (idxmap.containsKey(word)) continue;
                idxmap.put(word, size++);
            }
        }
        // 关联
        this.parent = new int[size];
        Arrays.fill(parent, -1);
        this.rootRecord = new HashMap<>();
        for (List<String> synonym : synonyms) {
            String word1 = synonym.get(0), word2 = synonym.get(1);
            int idx1 = idxmap.get(word1), idx2 = idxmap.get(word2);
            int root = union(find(idx1), find(idx2));
            
            // 记录不想交集元素
            if (rootRecord.get(root) == null) rootRecord.put(root, new TreeSet<>());
            rootRecord.get(root).add(word1);
            rootRecord.get(root).add(word2);
        }
    }

    public int union(int root1, int root2) {
        if (parent[root1] > 0 || parent[root2] > 0) throw new IllegalArgumentException("invalid param");

        if (root1 == root2) return root1;
        if (parent[root1] <= parent[root2]) {
            parent[root1] += parent[root2];
            parent[root2] = root1;
            return root1;
        } else {
            parent[root2] += parent[root1];
            parent[root1] = root2;
            return root2;
        }
    }

    public int find(int idx) {
        if (parent[idx] < 0) return idx;
        return parent[idx] = find(parent[idx]);
    }

    // 返回单个不相交集
    public TreeSet<String> getSynonyms(String word) {
        if (!idxmap.containsKey(word)) return null;
        return rootRecord.get(find(idxmap.get(word)));
    }
}
```

## 性能分析

&emsp;时间复杂度构建不相交集需要$O(N\log_2N)$，回溯需要$O(M^K\log_2N)$，其中$M$是待替换同义词数目，$K$为平均同义词数目。空间复杂度为$O(M^K)$。

执行用时：3ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.2MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。