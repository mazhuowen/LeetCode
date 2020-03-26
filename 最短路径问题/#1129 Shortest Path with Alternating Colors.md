[toc]

Consider a directed graph, with nodes labelled `0, 1, ..., n - 1`.  In this graph, each edge is either red or blue, and there could be self-edges or parallel edges.

Each `[i, j]` in `red_edges` denotes a red directed edge from node `i` to node `j`.  Similarly, each `[i, j]` in `blue_edges` denotes a blue directed edge from node `i` to node `j`.

Return an array `answer` of length n, where each `answer[X]` is the length of the shortest path from node `0` to node `X` such that the edge colors alternate along the path (or `-1` if such a path doesn't exist).



Constraints:

* $1 \le n \le 100$
* $\text{red_edges.length} \le 400$
* $\text{blue_edges.length} \le 400$
* $\text{red_edges[i].length} == \text{blue_edges[i].length} == 2$
* $0 \le \text{red_edges[i][j], blue_edges[i][j]} < n$



## 题目解读

&emsp;给定有向图，图中边分为红蓝两种颜色，且存在自环或平行边。路径只能是两种颜色交替出现，计算给出的点到其它结点的距离，如果不存在，则设为`-1`。

```java
class Solution {
    public int[] shortestAlternatingPaths(int n, int[][] red_edges, int[][] blue_edges) {

    }
}
```

## 程序设计

* 粗看是改进的有向图最短距离问题，在经典算法的结构上只需在更新距离时判断颜色是否交替，代码如下：

```java
class Solution {
    public int[] shortestAlternatingPaths(int n, int[][] red_edges, int[][] blue_edges) {
        if (n <= 0) throw new IllegalArgumentException("invalid param");

        Node[] graph = new Node[n];
        for (int[] edge : red_edges) {
            graph[edge[0]] = new Node(edge[1], 0, graph[edge[0]]);
        }
        for (int[] edge : blue_edges) {
            graph[edge[0]] = new Node(edge[1], 1, graph[edge[0]]);
        }

        // 距离和前驱，及记录路径颜色
        int[] distance = new int[n];
        int[] prev = new int[n];
        // 记录当前结点在路径中的入边的颜色
        int[] color = new int[n];
        boolean[] flag = new boolean[n];
        Arrays.fill(distance, -1);

        // 初始化起点，起点没有入边，颜色置为-1
        distance[0] = 0;
        prev[0] = 0;
        color[0] = -1;

        for (int i = 0; i < n - 1; i++) {
            // 查找当前的最小距离
            int minIdx = -1;
            for (int j = 0; j < n; j++) {
                if (!flag[j] && distance[j] != -1 && (minIdx == -1 || distance[j] < distance[minIdx])) minIdx = j;
            }
            // 无法连通剩余的结点，退出
            if (minIdx == -1) break;
            // 加入路径
            flag[minIdx] = true;

            int col = color[minIdx];
            // 遍历miniDx出边
            Node temp = graph[minIdx];
            while (temp != null) {
                if (!flag[temp.ver] && temp.color != col && (distance[temp.ver] == -1
                        || distance[temp.ver] > distance[minIdx] + 1)) {
                    distance[temp.ver] = distance[minIdx] + 1;
                    prev[temp.ver] = minIdx;
                    color[temp.ver] = temp.color;
                }

                temp = temp.next;
            }
        }

        return distance;
    }
}

class Node {
    int ver;
    int color;
    Node next;

    Node(int ver, int color, Node next) {
        this.ver = ver;
        this.color = color;
        this.next = next;
    }
}
```

* 上述思路不通过，测试用例：红边`[[0,1],[1,2],[2,3],[3,4]]`，蓝边`[[1,2],[2,3],[3,1]]`报错，结点`4`的路径需要经过`1,2,3,`的循环链路，而经典有向图最短路径算法是基于图中的权重都是正数的前提，从而每次贪心选择最短距离更新；在此题中存在循环链路，则上述算法不再合适。

* 联想到有向负权重图中采用穷举法遍历更新，本题可以采用相同思路。但是有向负权重图最短路径算法不能存在环，在本题中可以遍历完成后删除遍历的边，来解决环的问题。

```java
class Solution {
    public int[] shortestAlternatingPaths(int n, int[][] red_edges, int[][] blue_edges) {
        if (n <= 0) throw new IllegalArgumentException("invalid param");

        // 构建图
        Node[] graph = new Node[n];
        for (int[] edge : red_edges) {
            graph[edge[0]] = new Node(edge[1], 0, graph[edge[0]]);
        }
        for (int[] edge : blue_edges) {
            graph[edge[0]] = new Node(edge[1], 1, graph[edge[0]]);
        }

        // 距离
        int[] distance = new int[n];
        Arrays.fill(distance, -1);

        Queue<int[]> queue = new LinkedList<>();
        // 当前结点、累计路径长度、颜色
        queue.add(new int[]{0 ,0, -1});
        distance[0] = 0;

        while (!queue.isEmpty()) {
            int[] cur = queue.poll();

            int color = cur[2];
            Node next = graph[cur[0]];
            Node pre = null;
            while (next != null) {
                // 颜色交替
                if (color != next.color) {
                    // 更新
                    if (distance[next.ver] == -1 || distance[next.ver] > cur[1] + 1) distance[next.ver] = cur[1] + 1;
                    // 删除遍历过的边，避免死循环
                    if (pre == null) {
                        graph[cur[0]] = next.next;
                    } else {
                        pre.next = next.next;
                    }
                    queue.add(new int[]{next.ver, cur[1] + 1, next.color});
                }
                pre = next;
                next = next.next;
            }
        }

        return distance;
    }
}

class Node {
    int ver;
    int color;
    Node next;

    Node(int ver, int color, Node next) {
        this.ver = ver;
        this.color = color;
        this.next = next;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(V + E)$，空间复杂度为$O(V + E)$。

执行用时：5ms，在所有java提交中击败了95.76%的用户。

内存消耗：41.6MB，在所有java提交中击败了89.19%的用户。

## 官方解题

&emsp;暂无，密切关注。