[toc]

秋游中的小力和小扣设计了一个追逐游戏。他们选了秋日市集景区中的$N$个景点，景点编号为$1 \sim N$。此外，他们还选择了$N$条小路，满足任意两个景点之间都可以通过小路互相到达，且不存在两条连接景点相同的小路。整个游戏场景可视作一个无向连通图，记作二维数组`edges`，数组中以`[a,b]`形式表示景点`a`与景点`b`之间有一条小路连通。

小力和小扣只能沿景点间的小路移动。小力的目标是在最快时间内追到小扣，小扣的目标是尽可能延后被小力追到的时间。游戏开始前，两人分别站在两个不同的景点`startA`和`startB`。每一回合，小力先行动，小扣观察到小力的行动后再行动。小力和小扣在每回合可选择以下行动之一：

* 移动至相邻景点
* 留在原地

如果小力追到小扣（即两人于某一时刻出现在同一位置），则游戏结束。若小力可以追到小扣，请返回最少需要多少回合；若小力无法追到小扣，请返回$-1$。

注意：小力和小扣一定会采取最优移动策略。



提示：

* `edges`的长度等于图中节点个数
* $3 \le \text{edges.length} \le 10^5$
* $1 \le \text{edges[i][0], edges[i][1]} \le \text{edges.length}$ 且 $\text{edges[i][0]} \ne \text{edges[i][1]}$
* $1 \le \text{startA, startB} \le \text{edges.length}$ 且 $\text{startA} \ne \text{startB}$



## 题目解读

&emsp;给定无向图，求`A`追上`B`的最大步数，如果追不上，返回$-1$。

```java
class Solution {
    public int chaseGame(int[][] edges, int startA, int startB) {
        
    }
}
```

## 程序设计

* 题目规定$N$个节点有$N$个边，且节点两两之间连通，则必然只有一个环；
* 参考社区思路，对于`A`、`B`，让`A`追不上，则`B`必须尝试去圆上，有如下情况：
  * 如果`A`、`B`相邻，则`A`只需一步就可以追上`B`；
  * 环的长度为$2$，不管`A`、`B`是否在换上，最终都可以追上`B`；
  * 环的长度为$3$，即使`A`、`B`最终到达圆上，`A`、`B`间的最小距离总是$1$，`A`总可以在下个回合追上`B`；
  * 当环的长度大于等于$4$时，`B`需尽快到达圆的某个点，如果`A`不能提前到达该点，则`A`永远追不上`B`。
* 分析梳理后，如果环长度大于等于$4$，则需判断是否可能追不上；计算`B`到达圆的最短距离为$l$，则在`B`到达圆前，`A`必须提前到达圆上且距离`B`到达的点不能超过$1$，这样下回合`A`就可以追上`B`，即`A`到达该点的距离要少于等于$l + 1$，否则`B`会提前上圆并拉开与`A`的距离，`A`再也追不上。
* 对于环的长度小于$4$或`A`可以提前到达圆的情况，`A`肯定能追上`B`，此时需要计算在最优策略下的最大步数；同理，不管`B`到达哪个位置，如果`A`可以提前到，则都会追上`B`；为了最大化步数，`B`必须以最短距离去`A`无法提前到达的点，然后按兵不动等`A`追上来，最后步数就是`A`到达该点步数。
* 梳理上述过程，首先需要构图然后求`A`、`B`到其他节点的单源最短路径，由于是无权图，直接使用广度优先搜索计算；然后需要标记环并统计环的长度，可使用深度优先搜索遍历判断；最后根据环的长度比较，如果环的长度大于等于$4$，则需要再次遍历计算`B`到圆的最短距离然后和`A`比较；否则需要遍历所有`B`可提前`A`到达的点并记录最大距离。

```java
class Solution {
    public int chaseGame(int[][] edges, int startA, int startB) {
        // 构图
        Node[] graph = buildGraph(edges);
        // 计算两个人到其他节点的距离
        int[] disA = distance(startA - 1, graph);
        int[] disB = distance(startB - 1, graph);
        // A于B只相聚一个边
        if (disA[startB - 1] == 1) return 1;

        // 判断环
        int[] circle = new int[edges.length];
        int len = findCircle(graph, circle);

        // 环大于4，存在追不上的可能
        if (len >= 4) {
            // 寻找B到环的最短距离
            int[] toCircle = findToCircle(startB - 1, graph, circle);
            if (disA[toCircle[0]] > toCircle[1] + 1) return -1;
        }

        // A一定可以捉到B，B要使自己尽可能晚被捉到
        int res = 0;
        for (int i = 0; i < graph.length; i++) if (disA[i] > disB[i] + 1) res = Math.max(res, disA[i]);
        return res;

    }

    // 构图
    private Node[] buildGraph(int[][] edges) {
        Node[] graph = new Node[edges.length];
        for (int[] edge : edges) {
            int from = edge[0] - 1, to = edge[1] - 1;
            graph[from] = new Node(to, graph[from]);
            graph[to] = new Node(from, graph[to]);
        }
        return graph;
    }

    // 非加权图广度优先搜索计算距离
    private int[] distance(int ver, Node[] graph) {
        int[] dis = new int[graph.length];
        boolean[] visited = new boolean[graph.length];

        Queue<Integer> queue = new LinkedList<>();
        queue.add(ver);
        visited[ver] = true;
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            for (Node tmp = graph[cur]; tmp != null; tmp = tmp.next) {
                if (visited[tmp.ver]) continue;
                visited[tmp.ver] = true;
                dis[tmp.ver] = dis[cur] + 1;
                queue.add(tmp.ver);
            }
        }
        return dis;
    }

    // 深度优先搜索标记环，并返回环的长度
    private int findCircle(Node[] graph, int[] circle) {
        int res = 0;
        dfs(-1, 0, graph, circle);
        // 统计环的长度
        for (int flag : circle) if (flag == -1) res++;
        return res;
    }

    private boolean dfs(int pre, int cur, Node[] graph, int[] circle) {
        // 存在环，标记为终止节点
        if (circle[cur] == 1) {
            circle[cur] = -1;
            return true;
        }
        // 标记为正在访问
        circle[cur] = 1;

        boolean res = false;
        for (Node tmp = graph[cur]; tmp != null; tmp = tmp.next) {
            if (pre == tmp.ver || circle[tmp.ver] == -1) continue;
            // 当前节点为环中的一个节点（非终止节点）
            if (dfs(cur, tmp.ver, graph, circle) && circle[cur] != -1) {
                circle[cur] = -1;
                res = true;
            }
        }
        return res;
    }

    // 返回B到环的最短距离和节点
    private int[] findToCircle(int ver, Node[] graph, int[] circle) {
        int[] res = new int[2];
        int dis = 0;
        boolean[] visited = new boolean[graph.length];
        Queue<Integer> queue = new LinkedList<>();
        queue.add(ver);
        visited[ver] = true;
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int cur = queue.poll();
                if (circle[cur] == -1) {
                    res[0] = cur;
                    res[1] = dis;
                    return res;
                }
                for (Node tmp = graph[cur]; tmp != null; tmp = tmp.next) {
                    if (visited[tmp.ver]) continue;
                    visited[tmp.ver] = true;
                    queue.add(tmp.ver);
                }
            }
            dis++;
        }
        return res;
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

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：43ms，在所有java提交中击败了92.31%的用户。

内存消耗：114.6MB，在所有java提交中击败了53.85%的用户。

## 官方解题

&emsp;暂无，密切关注。