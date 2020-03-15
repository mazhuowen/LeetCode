[toc]

Two strings `X` and `Y` are similar if we can swap two letters (in different positions) of `X`, so that it equals `Y`. Also two strings `X` and `Y` are similar if they are equal.

For example, `"tars"` and `"rats"` are similar (swapping at positions `0` and `2`), and `"rats"` and `"arts"` are similar, but `"star"` is not similar to `"tars"`, `"rats"`, or `"arts"`.

Together, these form two connected groups by similarity: `{"tars", "rats", "arts"}` and `{"star"}`.  Notice that `"tars"` and `"arts"` are in the same group even though they are not similar.  Formally, each group is such that a word is in the group if and only if it is similar to at least one other word in the group.

We are given a list `A` of strings.  Every string in `A` is an anagram of every other string in `A`.  How many groups are there?

Constraints:

* $1 \le \text{A.length} \le 2000$
* $1 \le \text{A[i].length} \le 1000$
* $\text{A.length} * \text{A[i].length} \le 20000$
* All words in `A` consist of lowercase letters only.
* All words in `A` have the same length and are anagrams of each other.
* The judging time limit has been increased for this question.



## 题目解读

&emsp;给定不包含重复的字符串列表，列表中的每个字符串都是其它所有字符串的一个字母异位词。给出列表中有几组相近词。

```java
class Solution {
    public int numSimilarGroups(String[] A) {

    }
}
```

## 程序设计

* 最基本的思路是使用不相交集记录相近词。需要两层遍历单词并比较，然后需要遍历两个字符串。最后返回不相交集尺寸。

```java
class Solution {
    public int numSimilarGroups(String[] A) {
        if(A == null || A.length == 0) {
            return 0;
        }
        DisJoint disJoint = new DisJoint(A.length);
        // 遍历当前单词之后的单词是否是相近词
        for(int i = 0; i < A.length - 1; i++) {
            for(int j = i + 1;j < A.length; j++ ) {
                if(A[i].length() != A[j].length()) continue;
                // 是相似字符串，则加入不相交集
                if(isAnagram(A[i], A[j])) {
                    disJoint.union(disJoint.find(i), disJoint.find(j));
                }
            }
        }
        return disJoint.size();
    }

    private boolean isAnagram(String w1, String w2) {
        // 相等则返回
        if(w1.equals(w2)) return true;
        // 找到不相等的位置
        int start = 0, end = w1.length() - 1;
        while(w1.charAt(start) == w2.charAt(start)) start++;
        while(w1.charAt(end) == w2.charAt(end)) end--;
        // 交换值是否相等
        if(w1.charAt(start) != w2.charAt(end) || w1.charAt(end) != w2.charAt(start)) return false;
        // 交换值之间的值必须相等
        while(++start < end) {
            if(w1.charAt(start) != w2.charAt(start)) return false;
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
        // 初始化为size个不相交集
        for(int i = 0; i < size; i++) {
            tree[i] = -1;
        }
    }
    // 按规模并
    public void union(int root1, int root2) {
        if(tree[root1] >= 0 || tree[root2] >= 0) {
            throw new IllegalArgumentException("not a root");
        }
        if(root1 == root2) {
            return;
        }
        // 树2大，树1并入树2
        if(tree[root1] >= tree[root2]) {
            tree[root2] += tree[root1];
            tree[root1] = root2;
        } else {
            tree[root1] += tree[root2];
            tree[root2] = root1;
        }
        size--;
    }
    // 路径压缩
    public int find(int val) {
        if(tree[val] < 0) {
            return val;
        }
        return tree[val] = find(tree[val]);
    }

    public int size() {
        return size;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2M)$，空间复杂度为$O(N)$，其中$N$为单词数目，$M$为平均单词长度。

执行用时：576ms，在所有java提交中击败了37.93%的用户。

内存消耗：40.9MB，在所有java提交中击败了26.92%的用户。

## 官方解题

&emsp;除了上述暴力法，官方还提供了主动生成法，每个单词都主动生成相近词并加入不相交集。**这个方法的前提是`A`中的单词都是由相同的字符构成的**。主动生成法特别需要注意的是关联逻辑，比如`rats`可以生成`satr`，而`star`生成`satr`，但是这不代表`rats`和`star`是相近的，除非`A`中存在中间词`satr`，这一个逻辑非常关键。首先使用字典保存生成关系，字典键是生成的词字典值是`A`中的单词，这样遍历过程中发现字典中存在当前单词的键，表示可由之前的单词生成，关联这两个单词；还有一种情况是`A`中两个单词都可以生成某个词，就像上面分析的，需要判断这个词是否也在`A`中，在则关联。

> 此处有个逻辑问题就是，单词`a`生成`b`，如果`a`在`b`前面则会加入生成映射到字典，遍历到`b`时加入集合；但是若`b`在前面，而`a`在后面怎么办？遍历到`b`时`a->b`的逻辑还没生成。实际上一样的，`b`会生成`a`，所以前后没关系。

```java
class Solution {
    public int numSimilarGroups(String[] A) {
        if(A == null || A.length == 0) {
            return 0;
        }
        // 记录相近词和生成其的直接父节点（凡是记录到字典中的值，必须是A中的单词）
        Map<String, String> parents = new HashMap<>();
        // 记录单词及索引
        Map<String, Integer> idxMap = new HashMap<>();
        DisJoint disJoint = new DisJoint(A.length);
        // 遍历生成
        for(int i = 0; i < A.length; i++) {
            String cur = A[i];
            // 说明A存在重复单词，相同单词肯定近似，直接合并
            if(idxMap.get(cur) != null) {
                disJoint.union(disJoint.find(idxMap.get(cur)), disJoint.find(i));
                continue;
            }
            // 当前词存在转换关系，直接关联
            if(parents.get(cur) != null && idxMap.get(parents.get(cur)) != null) {
                disJoint.union(disJoint.find(idxMap.get(parents.get(cur))), disJoint.find(i));
            }
            idxMap.put(cur, i);
            // 遍历生成所有的相近词，j、k为交换位置
            for(int j = 0; j < cur.length() - 1; j++) {
                for(int k = j + 1; k < cur.length(); k++) {
                    String similar = productSimilar(cur, j, k);
                    // 表示A中有多个单词都可以转换为similar
                    if(parents.get(similar) != null) {
                        // 只有当A中存在中间词similar时，这两个词相近
                        if(parents.values().contains(similar)) {
                            disJoint.union(disJoint.find(idxMap.get(parents.get(similar))), disJoint.find(i));
                        }
                    }
                    // 维护生成关系
                    parents.put(similar, cur);
                }
            }
        }
        return disJoint.size();
    }

    private String productSimilar(String origin, int i, int j) {
        StringBuffer sb = new StringBuffer(origin);
        char temp = sb.charAt(i);
        sb.setCharAt(i, sb.charAt(j));
        sb.setCharAt(j, temp);
        return sb.toString();
    }
}

class DisJoint {
    private int size;
    private int[] tree;

    DisJoint(int size) {
        this.size = size;
        this.tree = new int[size];
        // 初始化为size个不相交集
        for(int i = 0; i < size; i++) {
            tree[i] = -1;
        }
    }

    // 按规模并
    public void union(int root1, int root2) {
        if(tree[root1] >= 0 || tree[root2] >= 0) {
            throw new IllegalArgumentException("not a root");
        }
        if(root1 == root2) {
            return;
        }
        // 树2大，树1并入树2
        if(tree[root1] >= tree[root2]) {
            tree[root2] += tree[root1];
            tree[root1] = root2;
        } else {
            tree[root1] += tree[root2];
            tree[root2] = root1;
        }
        size--;
    }
    // 路径压缩
    public int find(int val) {
        if(tree[val] < 0) {
            return val;
        }
        return tree[val] = find(tree[val]);
    }

    public int size() {
        return size;
    }
}
```

&emsp;时间复杂度为$O(NM^3)$，空间复杂度为$O(NM^2)$。运行报超过时间限制。官方将两种方案结合在一起，当$N > M^2$时，即单词较短时采用这个方案，否则采用上面的方案。