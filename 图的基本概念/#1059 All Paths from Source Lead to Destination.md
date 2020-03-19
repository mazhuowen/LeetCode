[toc]

Given the `edges` of a directed graph, and two nodes `source` and `destination` of this graph, determine whether or not all paths starting from `source` eventually end at `destination`, that is:

- At least one path exists from the `source` node to the `destination` node
- If a path exists from the `source` node to a node with no outgoing edges, then that node is equal to `destination`.
- The number of possible paths from `source` to `destination` is a finite number.

Return `true` if and only if all roads from `source` lead to `destination`.

**Note:**

* The given graph may have self loops and parallel edges.
* The number of nodes `n` in the graph is between `1` and `10000`
* The number of edges in the graph is between `0` and `10000`
* $0 \le \text{edges.length} \le 10000$
* $\text{edges[i].length} == 2$
* $0 \le \text{source} \le n - 1$
* $0 \le destination \le n - 1$



## 题目解读

&emsp;判断一个点的所有路径能否到指定终点。终点定义为没有出边的点。

```java
class Solution {
    public boolean leadsToDestination(int n, int[][] edges, int source, int destination) {

    }
}
```

## 程序设计

* 首先判断终点是否满足要求，遍历采用深度优先搜索，遇到不合法路径返回`false`。由题意，该路径无环，且终端是指定点。

```java
class Solution {
    public boolean leadsToDestination(int n, int[][] edges, int source, int destination) {
        if (n <= 0 || edges == null || edges.length == 0) return true;
        // 构建图
        Node[] graph = new Node[n];
        for (int[] edge : edges) {
            graph[edge[0]] = new Node(edge[1], graph[edge[0]]);
        }
        // 题目限定目标点必须是终结点，不能有出边
        if (graph[destination] != null) return false;
        // 标记访问
        int[] visited = new int[n];
        return dfs(source, destination, graph, visited);
    }

    private boolean dfs(int start, int end, Node[] graph, int[] visited) {
        // 存在环
        if (visited[start] == 1) return false;
        // 已经遍历过，不再遍历
        if (visited[start] == -1) return true;
        // 当前点就是终点
        if (start == end) {
            visited[start] = -1;
            return true;
        }
        // 标记为正在遍历
        visited[start] = 1;
        Node cur = graph[start];
        // 走到端点，但不是目标点
        if (cur == null) return false;
        while (cur != null) {
            // 子路径不满足要求
            if (!dfs(cur.ver, end, graph, visited)) return false;
            cur = cur.next;
        }
        // 遍历完成，满足要求
        visited[start] = -1;
        return true;
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

&emsp;时间复杂度为$O(V + E)$，空间复杂度为$O(V + E)$。

执行用时：4ms，在所有java提交中击败了100.00%的用户。

内存消耗：47.2MB，在所有java提交中击败了5.26%的用户。

## 官方解题

&emsp;暂无，密切关注。