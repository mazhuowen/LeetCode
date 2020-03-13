[toc]

In this problem, a tree is an **undirected** graph that is connected and has no cycles.

The given input is a graph that started as a tree with N nodes (with distinct values 1, 2, ..., N), with one additional edge added. The added edge has two different vertices chosen from `1` to `N`, and was not an edge that already existed.

The resulting graph is given as a 2D-array of `edges`. Each element of `edges` is a pair `[u, v]` with `u < v`, that represents an **undirected** edge connecting nodes `u` and `v`.

Return an edge that can be removed so that the resulting graph is a tree of N nodes. If there are multiple answers, return the answer that occurs last in the given 2D-array. The answer edge `[u, v]` should be in the same format, with `u < v`.

Note:

* The size of the input 2D-array will be between `3` and `1000`.
* Every integer represented in the 2D-array will be between `1` and `N`, where `N` is the size of the input array.



## 题目解读

&emsp;给定有$N$条边的无向图，假设删除一条边后是一棵树，找到这条边。

```java
class Solution {
    public int[] findRedundantConnection(int[][] edges) {

    }
}
```

## 程序设计

* 一棵$N$个结点的树有$N-1$条边，多一条边就会构成环路，转化到本题就是找到构成环路的那条边。由于是无向图，删除任意一个在环路中的边都可以变成一棵树，题目限定返回序列在最后的边。

```java
class Solution {
    public int[] findRedundantConnection(int[][] edges) {
        if(edges == null || edges.length == 0) {
            throw new IllegalArgumentException("invalid param");
        }
        int n = edges.length;
        DisJoint disJoint = new DisJoint(n + 1);
        for (int i = 0; i < n; i++) {
            int[] edge = edges[i];
            // 已经是连通的，若加上这条边形成环路
            if(disJoint.find(edge[0]) == disJoint.find(edge[1])) {
                return edge;
            }
            disJoint.union(disJoint.find(edge[0]), disJoint.find(edge[1]));
        }
        throw new RuntimeException("logic error");
    }
}

class DisJoint {
    private int[] tree;
    private int size;

    DisJoint(int size) {
        this.size = size;
        this.tree = new int[size];
        // 填充-1
        Arrays.fill(tree, -1);
    }

    public void union(int root1, int root2) {
        if(tree[root1] >= 0 || tree[root2] >= 0) throw new IllegalArgumentException("root must be negative");
        if(root1 == root2) return;
        if(tree[root1] < tree[root2]) {
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

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了99.32%的用户。

内存消耗：39.5MB，在所有java提交中击败了31.16%的用户。

## 官方解题

&emsp;除了不相交集，任然是图的遍历的思路。