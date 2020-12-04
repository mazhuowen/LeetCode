[toc]

Given a directed, acyclic graph of `N` nodes.  Find all possible paths from node `0` to node `N-1`, and return them in any order.

The graph is given as follows:  the nodes are 0, 1, ..., graph.length - 1.  `graph[i]` is a list of all nodes `j` for which the edge `(i, j)` exists.




**Note**:

* The number of nodes in the graph will be in the range `[2, 15]`.
* You can print different paths in any order, but you should keep the order of nodes inside one path.



## 题目解读

&emsp;求解到目标点的所有路径。

```java
class Solution {
    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {

    }
}
```

## 程序设计

* 由于是可达的，只需深度优先搜索即可。

```java
class Solution {
    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
        List<List<Integer>> res = new LinkedList<>();
        if(graph == null || graph.length == 0) return res;

        int n = graph.length;
        dfs(graph, 0, n - 1, new boolean[n], new LinkedList<>(), res);
        return res;
    }

    private void dfs(int[][] graph, int cur, int end, boolean[] visited, LinkedList<Integer> path, List<List<Integer>> res) {
        // 到达终点
        if (cur == end) {
            res.add(new LinkedList<>(path){{add(cur);}});
            return;
        }

        visited[cur] = true;
        // 加入路径
        path.add(cur);

        for (int node : graph[cur]) {
            if (visited[node]) continue;
            dfs(graph, node, end, visited, path, res);
        }

        visited[cur] = false;
        path.removeLast();
    }
}
```

## 性能分析

&emsp;最坏情况每个节点都相连，时间复杂度为$O(N2^N)$，空间复杂度为$O(N2^N)$。

执行用时：3ms，在所有java提交中击败了89.34%的用户。

内存消耗：43.6MB，在所有java提交中击败了33.33%的用户。

## 官方解题

&emsp;同上。