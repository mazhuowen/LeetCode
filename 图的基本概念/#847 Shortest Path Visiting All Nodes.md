[toc]

An undirected, connected graph of $N$ nodes (labeled $0, 1, 2, \cdots, N-1)$ is given as graph.

`graph.length = N`, and `j != i` is in the list `graph[i]` exactly once, if and only if nodes `i` and `j` are connected.

Return the length of the shortest path that visits every node. You may start and stop at any node, you may revisit nodes multiple times, and you may reuse edges.



**Note:**

* $1 \le \text{graph.length} \le 12$
* $0 \le \text{graph[i].length} < \text{graph.length}$



## 题目解读

&emsp;给定无向连通图，求从任意点出发遍历所有点的最短的路径。可重复遍历边和结点。

```java
class Solution {
    public int shortestPathLength(int[][] graph) {
        
    }
}
```

## 程序设计

* 由于是连通图遍历所有点的最短路径，不是单源或多源最短距离，不能使用图的最短距离算法进行计算。 
* 参考官方解题，采用广度优先搜索遍历路径（而不是结点）查找最短距离；路径可以描述为初始出发点及当前遍历过的顶点集合，广度搜索直到遍历完所有结点。 
* 由于顶点少于12个，可使用二进制编码来表示是否访问顶点。

```java
class Solution {
    public int shortestPathLength(int[][] graph) {
        if (graph == null) throw new IllegalArgumentException("invalid param");

        int n = graph.length;
        // 使用二进制位表示遍历过的结点
        int finalStat = (1 << n) - 1;

        // 记录路径是否访问过
        Set<String> set = new HashSet<>();
        Queue<Stat> queue = new LinkedList<>();
        // 加入每个顶点为起始的状态
        for (int i = 0; i < n; i++) queue.add(new Stat(i, 0, finalStat));

        int step = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                Stat curStat = queue.poll();

                // 找到最短路径
                if (curStat.isFinal()) return step;
                // 加入下一层结点
                for (int next : graph[curStat.curNode]) {
                    Stat nextStat = curStat.getNextStat(next);
                    // 已遍历该路径，继续
                    if (set.contains(nextStat.toString())) continue;
                    set.add(nextStat.toString());
                    queue.add(nextStat);
                }
            }
            step++;
        }
        return -1;
    }
}

class Stat {
    // 最终状态
    final int finalStat;
    // 遍历状态
    int preStat;
    // 当前结点
    int curNode;

    Stat(int curNode, int preStat, int finalStat) {
        this.preStat = preStat;
        this.curNode = curNode;
        this.finalStat = finalStat;
    }

    public boolean isFinal() {
        return (preStat | (1 << curNode)) == finalStat;
    }

    // 获取下一个状态
    public Stat getNextStat(int nextNode) {
        return new Stat(nextNode, preStat | (1 << curNode), finalStat);
    }

    @Override
    public String toString() {
        return "Stat{" +
                "preStat=" + preStat +
                ", curNode=" + curNode +
                '}';
    }
}
```

* 将集合可换为二维数组，时间性能更好：

```java
class Solution {
    public int shortestPathLength(int[][] graph) {
        if (graph == null) throw new IllegalArgumentException("invalid param");

        int n = graph.length;
        // 使用二进制位表示遍历过的结点
        int finalStat = (1 << n) - 1;

        // 记录路径是否访问过
        boolean[][] visited = new boolean[n][finalStat];
        Queue<Stat> queue = new LinkedList<>();
        // 加入每个顶点为起始的状态
        for (int i = 0; i < n; i++) queue.add(new Stat(i, 0, finalStat));

        int step = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                Stat curStat = queue.poll();

                // 找到最短路径
                if (curStat.isFinal()) return step;
                // 加入下一层结点
                for (int next : graph[curStat.curNode]) {
                    Stat nextStat = curStat.getNextStat(next);
                    // 已遍历该路径，继续
                    if (visited[nextStat.curNode][nextStat.preStat]) continue;
                    visited[nextStat.curNode][nextStat.preStat] = true;
                    queue.add(nextStat);
                }
            }
            step++;
        }
        return -1;
    }
}

class Stat {
    // 最终状态
    final int finalStat;
    // 遍历状态
    int preStat;
    // 当前结点
    int curNode;

    Stat(int curNode, int preStat, int finalStat) {
        this.preStat = preStat;
        this.curNode = curNode;
        this.finalStat = finalStat;
    }

    public boolean isFinal() {
        return (preStat | (1 << curNode)) == finalStat;
    }

    // 获取下一个状态
    public Stat getNextStat(int nextNode) {
        return new Stat(nextNode, preStat | (1 << curNode), finalStat);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N2^N)$，空间复杂度为$O(N2^N)$。

执行用时：169ms，在所有java提交中击败了5.50%的用户。

内存消耗：48.9MB，在所有java提交中击败了50.00%的用户。

&emsp;替换为二维数组后：

执行用时：10ms，在所有java提交中击败了52.50%的用户。

内存消耗：40.2MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;上述思路参考官方解题。