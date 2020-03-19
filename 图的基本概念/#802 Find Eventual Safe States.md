[toc]

In a directed graph, we start at some node and every turn, walk along a directed edge of the graph. If we reach a node that is terminal (that is, it has no outgoing directed edges), we stop.

Now, say our starting node is *eventually safe* if and only if we must eventually walk to a terminal node. More specifically, there exists a natural number `K` so that for any choice of where to walk, we must have stopped at a terminal node in less than `K` steps.

Which nodes are eventually safe? Return them as an array in sorted order.

The directed graph has `N` nodes with labels `0, 1, ..., N-1`, where `N` is the length of `graph`. The graph is given in the following form: `graph[i]` is a list of labels `j` such that `(i, j)` is a directed edge of the graph.



**Note:**

- `graph` will have length at most `10000`.
- The number of edges in the graph will not exceed `32000`.
- Each `graph[i]` will be a sorted list of different integers, chosen within the range `[0, graph.length - 1]`.



## 题目解读

&emsp;在有向图中从某个节点沿着图的有向边走，直到到达终点。安全状态定义为从该点出发结果有限次能够到达终点。题目要求给出满足条件的点的集合，按顺序排列。

```java
class Solution {
    public List<Integer> eventualSafeNodes(int[][] graph) {

    }
}
```

## 程序设计

* 从某个结点出发能够有限次步数到达终点，首先这个结点能够连通终点，其次这个结点路径上没有环。

```java
class Solution {
    public List<Integer> eventualSafeNodes(int[][] graph) {
        List<Integer> res = new LinkedList<>();
        // 不存在顶点
        if(graph == null || graph.length == 0) return res;

        int n = graph.length;
        // 标记访问，-1表示已遍历，无环，-2表示有环，0表示未遍历，1表示再遍历
        int[] visited = new int[n];

        for (int i = 0; i < n; i++) {
            if (visited[i] == 0) dfs(i, graph, visited);
        }
		// 根据结点顺序输出满足条件的点
        for (int i = 0; i < n; i++) {
            if (visited[i] == -1) res.add(i);
        }
        return res;
    }

    private int dfs(int start, int[][] graph, int[] visited) {
        // 有环
        if (visited[start] == 1) {
            visited[start] = -2;
        }
        // 已经有遍历结果，返回
        if (visited[start] != 0) {
            return visited[start];
        }
        // 标记为已访问
        visited[start] = 1;
        // 访问后继结点
        for (int i = 0; i < graph[start].length; i++) {
            int cur = graph[start][i];
            int result = dfs(cur, graph, visited);
            // 如果存在环，则设置当前路径状态为-2，不结束遍历，继续其他分支遍历
            if (result != -1) {
                visited[start] = result;
            }
        }
        // 完成遍历，后续路径不存在环，故此处还是i，赋值为-1，返回
        if (visited[start] == 1) visited[start] = -1;
        return visited[start];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(V + E)$，空间复杂度为$O(V + E)$。

执行用时：7ms，在所有java提交中击败了92.15%的用户。

内存消耗：47.7MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方还提出了拓扑排序的思路，需要对图结构做改动。