[toc]

Given an undirected `graph`, return `true` if and only if it is bipartite.

Recall that a graph is *bipartite* if we can split it's set of nodes into two independent subsets A and B such that every edge in the graph has one node in A and another node in B.

The graph is given in the following form: `graph[i]` is a list of indexes `j` for which the edge between nodes `i` and `j` exists. Each node is an integer between `0` and `graph.length - 1`. There are no self edges or parallel edges: `graph[i]` does not contain `i`, and it doesn't contain any element twice.



**Note:**

- `graph` will have length in range `[1, 100]`.
- `graph[i]` will contain integers in range `[0, graph.length - 1]`.
- `graph[i]` will not contain `i` or duplicate values.
- The graph is undirected: if any element `j` is in `graph[i]`, then `i` will be in `graph[j]`.



## 题目解读

&emsp;图的着色问题，判断是否满足边的两个端点颜色不同。图为不存在重复边（平行边）的无向图。

```java
class Solution {
    public boolean isBipartite(int[][] graph) {

    }
}
```

## 程序设计

* 实际是图的遍历的变体，在图的遍历中，需要数组记录结点是否访问过，在此处可用于记录是否遍历过，及遍历后分配的颜色。dfs实现如下：

```java
class Solution {
    public boolean isBipartite(int[][] graph) {
       if (graph == null || graph.length == 0) return true;

       int n = graph.length;
       // 结点着色，0未访问，1表示颜色1，2表示颜色2，相邻结点不能相同
       int[] color = new int[n];
        for (int i = 0; i < n; i++) {
            // 未遍历的点标记为颜色1，并开始你深度优先搜索
            if (color[i] == 0 && !dfs(i, graph, color, 1)) return false;
        }
        return true;
    }

    private boolean dfs(int start, int[][] graph, int[] color, int col) {
        // 遍历点已被访问，颜色不相符则返回false，相符返回true
        if (color[start] != 0) return color[start] == col;
        
        // 标记颜色
        color[start] = col;
        // 遍历后继
        for (int i = 0; i < graph[start].length; i++) {
            int cur = graph[start][i];
            // 1^3得到2，2^3得到1，路径上交替着色
            if (!dfs(cur, graph, color, col ^ 3)) return false;
        }
        return true;
    }
}
```

* bfs实现如下：

```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        if (graph == null || graph.length == 0) return true;

        int n = graph.length;
        // 结点着色，0未访问,1表示颜色1,2表示颜色2，相邻结点不能相同
        int[] color = new int[n];
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            // 已遍历，跳过
            if (color[i] != 0) continue;

            // 颜色初始为1，加入起始结点
            int col = 1;
            queue.add(i);
            while (!queue.isEmpty()) {
                // 遍历同层结点
                int size = queue.size();
                for (int j = 0; j < size; j++) {
                    int cur = queue.poll();
                    if (color[cur] != 0) {
                        // 着色冲突
                        if (color[cur] != col) return false;
                        // 已遍历，跳过
                        continue;
                    }
                    // 着色
                    color[cur] = col;
                    // 加入下一层结点
                    for (int k = 0; k < graph[cur].length; k++) {
                        queue.add(graph[cur][k]);
                    }
                }
                // 颜色转换
                col ^= 3;
            }
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(V + E)$，空间复杂度为$O(V + E)$。

执行用时：0 ms，在所有java提交中击败了100.00%的用户。

内存消耗：41.9MB，在所有java提交中击败了78.74%的用户。

&emsp;时间复杂度为$O(V + E)$，空间复杂度为$O(V + E)$。

执行用时：6ms，在所有java提交中击败了8.84%的用户。

内存消耗：41.3MB，在所有java提交中击败了78.74%的用户。

## 官方解题

&emsp;同上。