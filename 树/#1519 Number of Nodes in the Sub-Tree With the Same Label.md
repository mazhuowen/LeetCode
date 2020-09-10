[toc]

Given a tree (i.e. a connected, undirected graph that has no cycles) consisting of $n$ nodes numbered from $0$ to $n - 1$ and exactly $n - 1$ `edges`. The root of the tree is the node $0$, and each node of the tree has **a label** which is a lower-case character given in the string labels (i.e. The node with the number $i$ has the label `labels[i]`).

The `edges` array is given on the form `edges[i] = [ai, bi]`, which means there is an edge between nodes `ai` and `bi` in the tree.

Return an array of size $n$ where `ans[i]` is the number of nodes in the subtree of the `i`th node which have the same label as node $i$.

A subtree of a tree `T` is the tree consisting of a node in `T` and all of its descendant nodes.



**Constraints**:

* $1 \le n \le 10^5$
* $\text{edges.length} == n - 1$
* $\text{edges[i].length} == 2$
* $0 \le a_i, b_i < n$
* $a_i \ne b_i$
* $\text{labels.length} == n$
* `labels` is consisting of only of lower-case English letters.



## 题目解读

&emsp;给定树，每个节点都有一个标签，返回每个节点与其子树中标签相同的节点数目。

```java
class Solution {
    public int[] countSubTrees(int n, int[][] edges, String labels) {
        
    }
}
```

## 程序设计

* 若是二叉树，则只需前序遍历统计子树标签，然后合并统计结果；
* 由于题目中是广义树，可通过构造无向图来表示树，由于根是$0$，即只需从$0$开始深度遍历，递归统计子树标签。

```java
class Solution {
    int[] res;
    
    public int[] countSubTrees(int n, int[][] edges, String labels) {
        if (n <= 0) return new int[]{};
        
        Node[] graph = new Node[n];
        for (int[] edge : edges) {
            int from = edge[0], to = edge[1];
            
            graph[from] = new Node(to, labels.charAt(to), graph[from]);
            graph[to] = new Node(from, labels.charAt(from), graph[to]);
        }
        
        this.res = new int[n];
        dfs(graph, 0, labels, new boolean[n]);
        return res;
    }
    
    private int[] dfs(Node[] graph, int ver, String labels, boolean[] visited) {
        int[] counter = new int[26];
        counter[labels.charAt(ver) - 'a'] = 1;
        visited[ver] = true;
        for (Node tmp = graph[ver]; tmp != null; tmp = tmp.next) {
            if (visited[tmp.ver]) continue;
            int[] r = dfs(graph, tmp.ver, labels, visited);
            for (int i = 0; i < 26; i++) {
                counter[i] += r[i];
            }
        }
        res[ver] = counter[labels.charAt(ver) - 'a'];
        return counter;
    }
}

class Node {
    int ver;
    char label;
    
    Node next;
    
    Node(int ver, char label, Node next) {
        this.ver = ver;
        this.label = label;
        this.next = next;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(VE)$，空间复杂度为$O(VE)$。

执行用时：55ms，在所有java提交中击败了98.08%的用户。

内存消耗：476.2MB，在所有java提交中击败了22.87%的用户。

## 官方解题

&emsp;同上。