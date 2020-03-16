[toc]

Given strings `A` and `B` of the same length, we say `A[i]` and `B[i]` are equivalent characters. For example, if `A = "abc"` and `B = "cde"`, then we have `'a' == 'c'`, `'b' == 'd'`, `'c' == 'e'`.

Equivalent characters follow the usual rules of any equivalence relation:

* Reflexivity: `'a' == 'a'`
* Symmetry: `'a' == 'b'` implies `'b' == 'a'`
* Transitivity: `'a' == 'b'` and `'b' == 'c'` implies `'a' == 'c'`

For example, given the equivalency information from `A` and `B` above, `S = "eed"`, `"acd"`, and `"aab"` are equivalent strings, and `"aab"` is the lexicographically smallest equivalent string of `S`.

Return the lexicographically smallest equivalent string of `S` by using the equivalency information from `A` and `B`.

Note:

* String `A`, `B` and `S` consist of only lowercase English letters from `'a' - 'z'`.
* The lengths of string `A`, `B` and `S` are between 1 and 1000.
* String `A` and `B` are of the same length.



## 题目解读

&emsp;给定两个等价字符串，意味着两个字符串相同位置的字符是等价的。给定另一字符串，返回字典序最小的等价字符串。

```java
class Solution {
    public String smallestEquivalentString(String A, String B, String S) {

    }
}
```

## 程序设计

* 将每个位置上的字符对关联，需注意由于要按字典序排序，所以每个集合的根结点可以设为最小的字符，这需要改造不相交集的合并策略。

```java
class Solution {
    public String smallestEquivalentString(String A, String B, String S) {
        if(A == null || B == null || A.length() != B.length()) throw new IllegalArgumentException("invalid param");
        DisJoint disJoint = new DisJoint(26);
        // 合并字符对
        for (int i = 0; i < A.length(); i++) {
            disJoint.union(disJoint.find(A.charAt(i) - 'a'), disJoint.find(B.charAt(i) - 'a'));
        }
        // 查找根字符并拼接
        StringBuffer sb = new StringBuffer();
        for (int i = 0; i < S.length(); i++) {
            sb.append((char)(disJoint.find(S.charAt(i) - 'a') + 'a'));
        }
        return sb.toString();
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
    // 根据题意，按照字典序合并，即索引大的合并到索引小的下面
    public void union(int root1, int root2) {
        if(tree[root1] >= 0 || tree[root2] >= 0) throw new IllegalArgumentException("not a root");
        if(root1 == root2) return;

        // 树2大，树1并入树2
        if(root1 > root2) {
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
        if(tree[val] < 0) return val;
        return tree[val] = find(tree[val]);
    }

    public int size() {
        return size;
    }
}
```

## 性能分析

时间复杂度为$O(max(N, M))$，空间复杂度为$O(1)$，其中$M$是目标字符串的长度。

执行用时：3ms，在所有java提交中击败了59.09%的用户。

内存消耗：37.7MB，在所有java提交中击败了14.71%的用户。

## 官方解题

&emsp;暂无，密切关注。