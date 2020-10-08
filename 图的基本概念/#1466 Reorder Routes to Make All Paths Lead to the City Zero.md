[toc]

There are $n$ cities numbered from $0$ to $n-1$ and $n-1$ roads such that there is only one way to travel between two different cities (this network form a tree). Last year, The ministry of transport decided to orient the roads in one direction because they are too narrow.

Roads are represented by `connections` where `connections[i] = [a, b]` represents a road from city `a` to `b`.

This year, there will be a big event in the capital (city 0), and many people want to travel to this city.

Your task consists of reorienting some roads such that each city can visit the city 0. Return the **minimum** number of edges changed.

It's **guaranteed** that each city can reach the city 0 after reorder.



**Constraints**:

* $2 \le n \le 5 * 10^4$
* $\text{connections.length} == n-1$
* $\text{connections[i].length} == 2$
* $0 \le \text{connections[i][0], connections[i][1]} \le n-1$
* $\text{connections[i][0]} \ne \text{connections[i][1]}$



## 题目解读

&emsp;求最小的改变额边反向的数目，使得每个点都可到达$0$。

```java
class Solution {
    public int minReorder(int n, int[][] connections) {

    }
}
```

## 程序设计

* 由于是无环图，可构建无向图，其中方向与边的方向相同的辨识`flag`，从$0$开始深度遍历，遇到边是`flag`的，表示需要反转。

```java
class Solution {
    public int minReorder(int n, int[][] connections) {
        // 构图
        Node[] graph = new Node[n];
        for (int[] connection : connections) {
            int from = connection[0], to = connection[1];
            graph[from] = new Node(to, true, graph[from]);
            graph[to] = new Node(from, false, graph[to]);
        }
        boolean[] visited = new boolean[n];
        // 修改的数目
        return dfs(0, graph, visited);
    }

    private int dfs(int cur, Node[] graph, boolean[] visited) {
        int res = 0;
        visited[cur] = true;
        for (Node tmp = graph[cur]; tmp != null; tmp = tmp.next) {
            if (visited[tmp.ver]) continue;
            // 需反转
            if (tmp.flag) res++;
            res += dfs(tmp.ver, graph, visited);
        }
        return res;
    }
}

class Node {
    int ver;
    // 标识方向
    boolean flag;
    Node next;

    Node(int ver, boolean flag, Node next) {
        this.ver = ver;
        this.flag = flag;
        this.next = next;
    }
}
```

> 也可构图后广度优先搜索。

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：13ms，在所有java提交中击败了95.62%的用户。

内存消耗：52.7MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。