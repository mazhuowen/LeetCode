[toc]

Alice and Bob have an undirected graph of `n` nodes and 3 types of edges:

* Type 1: Can be traversed by Alice only.
* Type 2: Can be traversed by Bob only.
* Type 3: Can by traversed by both Alice and Bob.

Given an array edges where `edges[i] = [typei, ui, vi]` represents a bidirectional edge of type `typei` between nodes `ui` and `vi`, find the maximum number of edges you can remove so that after removing the edges, the graph can still be fully traversed by both Alice and Bob. The graph is fully traversed by Alice and Bob if starting from any node, they can reach all other nodes.

Return the maximum number of edges you can remove, or return $-1$ if it's impossible for the graph to be fully traversed by Alice and Bob.



**Constraints**:

* $1 \le n \le 10^5$
* $1 \le \text{edges.length} \le \min(10^5, 3 * n * (n-1) / 2)$
* $\text{edges[i].length} == 3$
* $1 \le \text{edges[i][0]} \le 3$
* $1 \le \text{edges[i][1]} < \text{edges[i][2]} \le n$
* All tuples `(typei, ui, vi)` are distinct.



## 题目解读

&emsp;给定无向图三种边，编号$1$、$2$分别只能被Alice和Bob遍历，编号$3$可被两个人遍历；求删除的最少的边，使得Alice和Bob都可以遍历所有节点。

```java
class Solution {
    public int maxNumEdgesToRemove(int n, int[][] edges) {

    }
}
```

## 程序设计

* 如果不考虑三种边，则问题就是无向图的连通性，需使用不相交集判断；考虑到三种边，由于第三种边是通用的，首先使用不相交集判断删除多余的构成环的边，然后依次遍历类型$1$、$2$的边，并进行判断，最后判断不相交集是否是一个，不是说明无法连通，返回$-1$。

```java
class Solution {
    public int maxNumEdgesToRemove(int n, int[][] edges) {
        int res = 0;
        // 针对Alice和Bob，构建两个不相交集
        DisJoint d1 = new DisJoint(n), d2 = new DisJoint(n);
        // 公用边
        for (int[] edge : edges) {
            if (edge[0] != 3) continue;
            int from = edge[1] - 1, to = edge[2] - 1;
            int r1 = d1.find(from), r2 = d1.find(to);
            // 构成环，多余，删除
            if (r1 == r2) res++;
            // 连通
            else {
                d1.union(r1, r2);
                d2.union(r1, r2);
            }
        }
        
        // 依次判断Alice和Bob的边
        for (int[] edge : edges) {
            if (edge[0] == 3) continue;
            int type = edge[0], from = edge[1] - 1, to = edge[2] - 1;
            if (type == 1) {
                int r1 = d1.find(from), r2 = d1.find(to);
                if (r1 == r2) res++;
                else d1.union(r1, r2);
            } else {
                int r1 = d2.find(from), r2 = d2.find(to);
                if (r1 == r2) res++;
                else d2.union(r1, r2);
            }
        }
        
        // 判断是否连通
        return d1.size == 1 && d2.size == 1 ? res : -1;
    }
    
}

class DisJoint {
    int size;
    int[] parent;
    
    DisJoint(int size) {
        this.size = size;
        this.parent = new int[size];
        Arrays.fill(parent, -1);
    }
    
    public void union(int r1, int r2) {
        if (parent[r1] >= 0 || parent[r2] >= 0) throw new IllegalArgumentException("invalid param");
        if (r1 == r2) return;
        
        if (parent[r1] <= parent[r2]) {
            parent[r1] += parent[r2];
            parent[r2] = r1;
        } else {
            parent[r2] += parent[r1];
            parent[r1] = r2;
        }
        size--;
    }
    
    public int find(int idx) {
        if (parent[idx] < 0) return idx;
        return parent[idx] = find(parent[idx]);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：12ms，在所有java提交中击败了94.37%的用户。

内存消耗：80.9MB，在所有java提交中击败了68.84%的用户。

## 官方解题

&emsp;暂无，密切关注。