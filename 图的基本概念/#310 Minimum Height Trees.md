[toc]

For an undirected graph with tree characteristics, we can choose any node as the root. The result graph is then a rooted tree. Among all possible rooted trees, those with minimum height are called minimum height trees (MHTs). Given such a graph, write a function to find all the MHTs and return a list of their root labels.

**Format**
The graph contains `n` nodes which are labeled from `0` to `n - 1`. You will be given the number `n` and a list of undirected `edges` (each edge is a pair of labels).

You can assume that no duplicate edges will appear in `edges`. Since all edges are undirected, `[0, 1]` is the same as `[1, 0]` and thus will not appear together in `edges`.



**Note**:

- According to the definition of tree on Wikipedia: “a tree is an undirected graph in which any two vertices are connected by *exactly* one path. In other words, any connected graph without simple cycles is a tree.”
- The height of a rooted tree is the number of edges on the longest downward path between the root and a leaf.



## 题目解读

&emsp;给定无向无环图，找到一个结点使得无向图的树结构最低。

```java
class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {

    }
}
```

## 程序设计

* 首先想到的是以每一个顶点为起点，使用广度优先遍历记录层次并比较。但这个暴力搜索方法时间复杂度为$O(N^2)$，会超时。

```java
class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        List<Integer> res = new LinkedList<>();
        // 无效参数
        if (n <= 0) return res;
        // 无边图，每个结点都是最小高度树
        if (edges == null || edges.length == 0) {
            for (int i = 0; i < n; i++) {
                res.add(i);
            }
            return res;
        }

        // 构建图
        Node[] graph = new Node[n];
        for (int[] edge : edges) {
            graph[edge[0]] = new Node(edge[1], graph[edge[0]]);
            graph[edge[1]] = new Node(edge[0], graph[edge[1]]);
        }

        int minDep = Integer.MAX_VALUE;
        Queue<Integer> queue = new LinkedList<>();
        // 从每个结点开始广度优先遍历
        for (int i = 0; i < n; i++) {
            // 加入起始结点
            queue.add(i);
            int curDep = 0;
            boolean[] visited = new boolean[n];
            while (!queue.isEmpty()) {
                // 遍历同一高度的结点
                int size = queue.size();
                for (int j = 0; j < size; j++) {
                    int idx = queue.poll();
                    if (visited[idx]) continue;
                    // 置为已访问
                    visited[idx] = true;
                    // 加入下一层结点
                    Node temp = graph[idx];
                    while (temp != null) {
                        if (!visited[temp.val]) queue.add(temp.val);
                        temp = temp.next;
                    }
                }
                curDep++;
            }
            // 更新层数
            if (curDep == minDep) {
                res.add(i);
            } else if (curDep < minDep) {
                minDep = curDep;
                res.clear();
                res.add(i);
            }
        }
        return res;
    }
}

class Node {
    int val;
    Node next;

    Node(int val, Node next) {
        this.val = val;
        this.next = next;
    }
}
```

* 仔细观察，树根选择在图的中心时，树的整体高度较低；而对于一个无环无向图，总是会存在度为1的结点；度为1的结点对应叶节点，可以利用拓扑排序的思路，统计所有结点的度，然后将度为1的结点入队，此时表示这是同一层结点遍历这层结点的邻接点，如果删除这个结点，邻接点度为变为1，则入队；重复直到队列为空。

> 最后的根只能是１个或２个，如果大于２个则存在环路。

```java
class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        List<Integer> res = new LinkedList<>();
        // 无效参数
        if (n <= 0) return res;
        // 无边图，每个结点都是最小高度树
        if (edges == null || edges.length == 0) {
            for (int i = 0; i < n; i++) {
                res.add(i);
            }
            return res;
        }

        // 构建图
        Node[] graph = new Node[n];
        // 记录度
        int[] degree = new int[n];
        for (int[] edge : edges) {
            graph[edge[0]] = new Node(edge[1], graph[edge[0]]);
            graph[edge[1]] = new Node(edge[0], graph[edge[1]]);
            degree[edge[0]]++;
            degree[edge[1]]++;
        }
        // 初始化队列，加入度为1的点
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            if (degree[i] == 1) queue.add(i);
        }
        // 如果存在环路，则所有结点的度大于1
        if (queue.isEmpty()) throw new IllegalArgumentException("graph is not a DAG");

        while (!queue.isEmpty()) {
            // 清除上一层的数据
            res.clear();
            // 删掉最外层
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int cur = queue.poll();
                // 加入本层结点
                res.add(cur);
                Node temp = graph[cur];
                // 加入新的最外层结点
                while (temp != null) {
                    if (--degree[temp.val] == 1) queue.add(temp.val);
                    temp = temp.next;
                }
            }
        }
        // 最后res记录的就是最内层结点，也就是可能的根节点
        return res;
    }
}

class Node {
    int val;
    Node next;

    Node(int val, Node next) {
        this.val = val;
        this.next = next;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(V +E)$，空间复杂度为$O(V)$。

执行用时：8ms，在所有java提交中击败了97.33%的用户。

内存消耗：43.8MB，在所有java提交中击败了99.15%的用户。

## 官方解题

&emsp;暂无，密切关注。