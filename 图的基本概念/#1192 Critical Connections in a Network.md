[toc]

There are `n` servers numbered from `0` to `n-1` connected by undirected server-to-server `connections` forming a network where `connections[i] = [a, b]` represents a connection between servers `a` and `b`. Any server can reach any other server directly or indirectly through the network.

A critical connection is a connection that, if removed, will make some server unable to reach some other server.

Return all critical connections in the network in any order.



Constraints:

* $1 \le n \le 10^5$
* $n-1 \le \text{connections.length} \le 10^5$
* $\text{connections[i][0]} \ne \text{connections[i][1]}$
* There are no repeated connections.



## 题目解读

&emsp;关键连接是在集群中的重要连接，如果将它移除便会导致某些服务器无法访问其他服务器。返回该集群内的所有关键连接。

```java
class Solution {
    public List<List<Integer>> criticalConnections(int n, List<List<Integer>> connections) {

    }
}
```

## 程序设计

* 采用`tarjan`算法的变形来计算图中的桥。

> 原生`tarjan`是求解有向图的强连通分量的算法，此处将无向图构成双向图，然后利用算法的标号来判断分割点和桥，注意只有无向图可以判断，对有向图不适用。

```java
class Solution {
    int idx;
    List<List<Integer>> res;

    public List<List<Integer>> criticalConnections(int n, List<List<Integer>> connections) {
        // 初始编号为1
        this.idx = 1;
        this.res = new LinkedList<>();
        if (connections == null || connections.isEmpty()) return res;

        // 构图
        Node[] graph = new Node[n];
        for (List<Integer> edge : connections) {
            graph[edge.get(0)] = new Node(edge.get(1), graph[edge.get(0)]);
            graph[edge.get(1)] = new Node(edge.get(0), graph[edge.get(1)]);
        }

        int[] index = new int[n];
        int[] lowlink = new int[n];
        boolean[] visited = new boolean[n];

        for (int i = 0; i < n; i++) {
            if (visited[i]) continue;
            tarjan(i, -1, graph, visited, index, lowlink);
        }
        return res;
    }

    private void tarjan(int cur, int pre, Node[] graph, boolean[] visited, int[] index, int[] lowlink) {
        visited[cur] = true;
        // 赋予dfs搜索编号
        index[cur] = lowlink[cur] = idx++;

        Node next = graph[cur];
        while (next != null) {
            // 无向图两条边判断，避免造成死循环
            if (next.ver == pre) {
                next = next.next;
                continue;
            }

            // 未访问
            if (!visited[next.ver]) {
                // 深度有限搜索遍历标记编号
                tarjan(next.ver, cur, graph, visited, index, lowlink);
                // 更新可达的最低标记
                lowlink[cur] = Math.min(lowlink[cur], lowlink[next.ver]);

                // 发现分割点，lowlink[child] <= index[cur]说明当前节点和子结点在某个环内
                if (lowlink[next.ver] > index[cur]) {
                    List<Integer> temp = new ArrayList<>();
                    temp.add(cur);
                    temp.add(next.ver);
                    res.add(temp);
                }
            }
            // 已访问形成环路，将当前最低可达与后继标记判断，确定强可达的根
            else {
                // 不能和lowlink[next.ver]比较，因为下一个节点可以是其他环的一部分，这个需要记录当前环的根
                // 否则会变为其他环的根
                lowlink[cur] = Math.min(lowlink[cur], index[next.ver]);
            }

            next = next.next;
        }
        // 强连通分量的根，可利用栈保存节点，在此处将当前连通分量的节点输出
        // if (index[cur] == lowlink[cur])
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

&emsp;时间复杂度为$O(V + E)$，空间复杂度为$O(V)$。

执行用时：82ms，在所有java提交中击败了99.12%的用户。

内存消耗：107.8MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。