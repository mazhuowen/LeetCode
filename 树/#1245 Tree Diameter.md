[toc]

Given an undirected tree, return its diameter: the number of **edges** in a longest path in that tree.

The tree is given as an array of `edges` where `edges[i] = [u, v]` is a bidirectional edge between nodes `u` and `v`.  Each node has labels in the set `{0, 1, ..., edges.length}`.

 

**Example 1**:

<img src="..\images\#1245_exp1.png"  />

```
Input: edges = [[0,1],[0,2]]
Output: 2
Explanation: 
A longest path of the tree is the path 1 - 0 - 2.
```

**Example 2**:

<img src="..\images\#1245_exp2.png"  />

```
Input: edges = [[0,1],[1,2],[2,3],[1,4],[4,5]]
Output: 4
Explanation: 
A longest path of the tree is the path 3 - 2 - 1 - 4 - 5.
```



**Constraints**:

* $0 \le \text{edges.length} < 10^4$
* $\text{edges[i][0]} \ne \text{edges[i][1]}$
* $0 \le \text{edges[i][j]} \le \text{edges.length}$
* The given edges form an undirected tree.



## 题目解读

&emsp;给定无向树，求解节点间路径最长的距离。

```java
class Solution {
    public int treeDiameter(int[][] edges) {

    }
}
```

## 程序设计

* 如果没有数据规模限制，完全可以计算每个节点到其他节点的距离，选择最长距离；
* 由于是无向树，必然连通无环，且可将任意节点作为根节点，这样类似于二叉树的最长路径，每次只需统计子树中最长的两条路径，然后返回当前节点的最长路径。

```java
class Solution {
    int max = 0;

    public int treeDiameter(int[][] edges) {
        int n = edges.length + 1;
        // 构图
        Node[] graph = new Node[n];
        for (int[] edge : edges) {
            int from = edge[0], to = edge[1];
            graph[from] = new Node(to, graph[from]);
            graph[to] = new Node(from, graph[to]);
        }

        // 保存当前节点到叶节点的最长路径
        int[] maxLen = new int[n];
        Arrays.fill(maxLen, -1);
        dfs(0, -1, graph, maxLen);
        return max;
    }

    private int dfs(int ver, int parent, Node[] graph, int[] maxLen) {
        if (maxLen[ver] != -1) return maxLen[ver];
        // 子树中最大和次大路径
        int first = -1, second = -1;
        for (Node tmp = graph[ver]; tmp != null; tmp = tmp.next) {
            if (tmp.ver == parent) continue;
            int child = dfs(tmp.ver, ver, graph, maxLen);
            if (child > first) {
                second = first;
                first = child;
            } else if (child > second) second = child;
        }
        max = Math.max(max, first + second + 2);
        return maxLen[ver] = first + 1;
    }
}

class Node {
    int ver;
    Node next;

    Node(int ver, Node next) {
        this.ver = ver;
        this.next = next;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：4 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：39.2 MB, 在所有 Java 提交中击败了98.54%的用户。

## 官方解题

&emsp;暂无，密切关注。