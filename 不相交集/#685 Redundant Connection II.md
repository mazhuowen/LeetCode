[toc]

In this problem, a rooted tree is a **directed** graph such that, there is exactly one node (the root) for which all other nodes are descendants of this node, plus every node has exactly one parent, except for the root node which has no parents.

The given input is a directed graph that started as a rooted tree with N nodes (with distinct values 1, 2, ..., N), with one additional directed edge added. The added edge has two different vertices chosen from 1 to N, and was not an edge that already existed.

The resulting graph is given as a 2D-array of edges. Each element of edges is a pair `[u, v]` that represents a **directed** edge connecting nodes u and v, where `u` is a parent of child `v`.

Return an edge that can be removed so that the resulting graph is a rooted tree of N nodes. If there are multiple answers, return the answer that occurs last in the given 2D-array.



**Note**:

* The size of the input 2D-array will be between 3 and 1000.
* Every integer represented in the 2D-array will be between 1 and N, where N is the size of the input array.



## 题目解读

&emsp;给定一棵树和额外的边构成的有向图，找到这条额外的边。如果存在多条符合条件的边，返回最后一条。

```java
class Solution {
    public int[] findRedundantDirectedConnection(int[][] edges) {

    }
}
```

## 程序设计

* 首先分析构成环路的情况，对于树，除了根，每个结点只有一个父节点，即入度为$1$；这样如果存在环路，必然要连接到其它结点，入度必然会增大。由于树的性质，如果接入的点是根，则根的入度增加为$1$，接入的是其它结点，则入度增加为$2$。
* 对于连接根的环路，实际上删除任意一个在环路上的边都符合要求，题目要求是遍历的最后一条边，实际就是不相交集遍历判断，最后形成环路的那条边就是答案。
* 对于接入非根结点的情况，由于当前结点入度是$2$，要恢复为一棵树，必须删除这两条边中的一个，且保证删除后没有环路。总结为找到这两条边，然后判断是否在环路上，将在环路上的边删除。这里要考虑特殊情况，即这两条边都在环路上，比如`1->2,1->3,2->3`，`3`的入度为$2$，且都在环路上，这时根据题意，需要返回后遍历的那条边，可以以后遍历的边为基准，重新遍历边的集合并合并比较，如果与后遍历的边形成环路，不管前面那条边是否在环路，返回后遍历的边；否则前面遍历的边必然在环路，返回即可。
* 在判断环路时，普通判断是先判断再加入集合，此时当前边就是补上环路的最后一块拼图，因为最后一个形成环路的边的点必然都在集合中，先加入则失去判断意义；而入度为$2$的边判断则是先将当前边加入集合再判断，此时这条判断的边是补上环路的最后拼图，需要先将当前边加入再判断。

```java
class Solution {
    public int[] findRedundantDirectedConnection(int[][] edges) {
        if(edges == null || edges.length == 0) {
            return new int[]{};
        }
        int n = edges.length;
        int[] parents = new int[n + 1];
        boolean flag = false;
        int[] e1 = {}, e2 = {};
        // 记录入度
        for(int[] edge : edges) {
            // 入度为2，存在两个父节点
            if(parents[edge[1]] != 0) {
                flag = true;
                e1 = new int[]{parents[edge[1]], edge[1]};
                e2 = edge;
                break;
            }
            parents[edge[1]] = edge[0];
        }
        // 查找环路
        DisJoint disJoint = new DisJoint(n + 1);
        for(int[] edge : edges) {
            if(flag) {
                // 先加入当前边，避免e2为最后环的一条边，直到遍历结束都没连通，导致错误
                if(edge[0] == e2[0] && edge[1] == e2[1]) continue;
                disJoint.union(disJoint.find(edge[0]), disJoint.find(edge[1]));
                // 以e2为参照，比较是否是环路的一部分，是则返回
                if(disJoint.find(e2[0]) == disJoint.find(e2[1])) return e2;
            } else {
                // 以当前点为参照，比较是否是环路的一部分，是则返回
                if(disJoint.find(edge[0]) == disJoint.find(edge[1])) return edge;
                disJoint.union(disJoint.find(edge[0]), disJoint.find(edge[1]));
            }
        }
        return e1;
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
        if(tree[root1] >= 0 || tree[root2] >= 0) throw new IllegalArgumentException("root must be nagetive");
        if(root1 == root2) return;
        if(tree[root1] <= tree[root2]) {
            tree[root1] += tree[root2];
            tree[root2] = root1;
        } else{
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

&emsp;时间复杂度为$O(N)$，空间复杂度为为$O(N)$。

执行用时：2ms，在所有java提交中击败了45.63%的用户。

内存消耗：39.7MB，在所有java提交中击败了78.85%的用户。

## 官方解题

&emsp;暂无，密切关注。