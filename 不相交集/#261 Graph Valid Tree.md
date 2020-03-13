[toc]

Given `n` nodes labeled from `0` to `n - 1` and a list of undirected edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

Note: you can assume that no duplicate edges will appear in `edges`. Since all edges are undirected, `[0,1]` is the same as `[1,0]` and thus will not appear together in edges.



## 题目解读

&emsp;判断一个无向图是否是一棵树。

```java
class Solution {
    public boolean validTree(int n, int[][] edges) {

    }
}
```

## 程序设计

* 对于一棵树，首先是强连通的，其次是无环的。

```java
class Solution {
    public boolean validTree(int n, int[][] edges) {
        if(n <= 0 || edges == null) {
            return false;
        }
        DisJoint disJoint = new DisJoint(n);
        for(int i = 0; i < edges.length; i++) {
            int root1 = disJoint.find(edges[i][0]);
            int root2 = disJoint.find(edges[i][1]);
            // 存在环路
            if(root1 == root2) {
                return false;
            } else {
                disJoint.union(root1, root2);
            }
        }
        // 是否连通
        return disJoint.getSize() == 1;
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
        // 合并后集合数目减少
        size--;
    }
    // 路径压缩
    public int find(int val) {
        if(tree[val] < 0) return val;
        return tree[val] = find(tree[val]);
    }
    // 返回集合数目
    public int getSize() {
        return size;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了95.88%的用户。

内存消耗：41.1MB，在所有java提交中击败了85.19%的用户。

## 官方解题

&emsp;暂无，密切关注。社区还有利用图的遍历搜索解决的思路，首先需要矩阵或链表记录图，然后需要数组记录可达的结点，最后还要遍历查看是否连通，总之思路不是最优。