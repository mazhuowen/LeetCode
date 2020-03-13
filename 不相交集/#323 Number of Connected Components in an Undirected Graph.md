[toc]

Given n nodes labeled from $0$ to $n - 1$ and a list of undirected edges (each edge is a pair of nodes), write a function to find the number of connected components in an undirected graph.

Note:
You can assume that no duplicate edges will appear in `edges`. Since all edges are undirected, `[0, 1]` is the same as `[1, 0]` and thus will not appear together in edges.



## 题目解读

&emsp;给定无向图的边，找出图中连通的数目。

```java
class Solution {
    public int countComponents(int n, int[][] edges) {

    }
}
```

## 程序设计

* 可以用不相交集的思路，每次合并边的端点，最后统计有几个集合即可。

```java
class Solution {
    public int countComponents(int n, int[][] edges) {
        if(edges == null || edges.length == 0) {
            return n < 0 ? 0 : n;
        }
        DisJoint disJoint = new DisJoint(n);
        for(int[] edge : edges) {
            disJoint.union(disJoint.find(edge[0]), disJoint.find(edge[1]));
        }
        return disJoint.size();
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

&emsp;时间复杂度为$O(N +Ｋ)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了81.38%的用户。

内存消耗：41.1MB，在所有java提交中击败了88.71%的用户。

## 官方解题

&emsp;暂无，密切关注。社区除了不相交集，还有图的遍历搜索思路，需要由边构建图的表示，然后遍历生成树，最后有几棵树就有几个连通分量。