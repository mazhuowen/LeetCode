[toc]

又到了一年一度的春游时间，小吴计划去游乐场游玩$1$天，游乐场总共有$N$个游乐项目，编号从$0$到$N-1$。小吴给每个游乐项目定义了一个非负整数值`value[i]`表示自己的喜爱值。两个游乐项目之间会有双向路径相连，整个游乐场总共有$M$条双向路径，保存在二维数组`edges`中。小吴计划选择一个游乐项目`A`作为这一天游玩的重点项目。上午小吴准备游玩重点项目`A`以及与项目`A`相邻的两个项目`B`、`C`（项目`A`、`B`与`C`要求是不同的项目，且项目`B`与项目`C`要求相邻），并返回`A`，即存在一条`A-B-C-A`的路径。下午，小吴决定再游玩重点项目`A`以及与`A`相邻的两个项目 `B'`、`C'`，（项目`A`、`B'`与`C'`要求是不同的项目，且项目`B'`与项目`C'`要求相邻），并返回`A`，即存在一条`A-B'-C'-A`的路径。下午游玩项目`B'`、`C'`可与上午游玩项目`B`、`C`存在重复项目。小吴希望提前安排好游玩路径，使得喜爱值之和最大。请你返回满足游玩路径选取条件的最大喜爱值之和，如果没有这样的路径，返回0。注意：一天中重复游玩同一个项目并不能重复增加喜爱值了。例如：上下午游玩路径分别是`A-B-C-A`与`A-C-D-A`那么只能获得`value[A] + value[B] + value[C] + value[D]`的总和。



**限制**：

* $3 \le \text{value.length} \le 10000$
* $1 \le \text{edges.length} \le 10000$
* $0 \le \text{edges[i][0],edges[i][1]} < \text{value.length}$
* $0 \le \text{value[i]} \le 10000$
* `edges`中没有重复的边
* $\text{edges[i][0]} \ne \text{edges[i][1]}$



## 题目解读

&emsp;对任意点`A`，找到`ABC`和`AB'C'`使得权重最大。

```java
class Solution {
    public int maxWeight(int[][] edges, int[] value) {

    }
}
```

## 程序设计

* 参考社区思路，问题可以拆解为寻找所有的三角形及找出最大权值的三角形；对于查找三角形，如果不改变原来的无向图结构，则遍历每个边，然后判断与该边两个端点相连的顶点，该顶点与当前边构成三角形；时间复杂度为$O(MN)$，会超时，而且由于无向图，遍历三角形的边会得到重复的三角形。
* 优化上述暴力搜索，将无向图根据出边变为有向图，出度低的节点指向出度高的节点，这样遍历边时只需要遍历出度较低的边。由于$\sum_i^N\text{Degree}_i = 2 \times M$，在原无向图中度数高于$\sqrt{M}$的边最多有$2\sqrt{M}$个，在经过有向化后每个点的出度不超过$2\sqrt{M}$，遍历$M$条边，两个端点分别遍历顶点不超过$2\sqrt{M}$，即时间复杂度为$O(4M\sqrt{M})$，而且得到的三角形是不重复的。这样在遍历每个三角形后记录三个顶点及分别对应的对边即可。

<img src="..\images\#lcp16_1.png"  />

* 完成寻找三角形后，开始查找最大的三角形组合。一般化，两个三角形组合可以看作是一个顶点与两条对边的组合，可分为只有一条对边（当前顶点只有一个三角形）、有两条对边，对标端点有重合（及两个三角形存在一条重复边）、两条对边无重合（两个三角形只存在重合顶点）。在得到边及对应的顶点后，可遍历所有的顶点，尝试该顶点的对边两两组合的情况；假设平均每个顶点有$K$个对边，则时间复杂度为$NK^2$，在$K$比较大时会超时。
* 对于每个顶点，如果第一大和第二大的对边无重合，则这个组合必然是当前顶点的最优组合；如果这两条边有重复端点呢？显然需要继续尝试；将第$i$大的边记为$l_i$，如果边的顺序是逆序的，假设当前边为$l_i$，则需要遍历$i + 1 \sim k$个边与当前边的组合，假设$i + 1 \sim j$的边与$l_i$存在重合，$l_j$与$l_i$不重合，则后续$j + 1 \sim k$无需遍历，因为重合组合$l_i$与$l_{i + 1}$必然是最优组合，不重复组合$l_i$与$l_j$是最优组合，这样就得到了第一个优化；当尝试完$l_i$与$i + 1 \sim k$的组合，接下来尝试$l_{i + 1}$与$i + 2 \sim k$的组合，由于$l_i$与$l_j$是最优无重复组合，不管$l_{i + 1}$与$l_j$是否重复，其组合必然小于$l_i$和$l_j$，故下一轮遍历可提前截止到$j - 1$，这样得到了第二个优化。
* 有了上述剪枝优化，在最坏情况下复杂度依然是$NK^2$。再次分析，如果$l_1$与$l_2$不重合，必然是最优解，结束尝试；$l_1$与$l_2$重合，则继续尝试，假设当前最优组合为$l_1$与$l_i$；继续尝试$l_2$，假设$l_2$与$l_3$不重复，是最优组合，则结束尝试，最优解就是$l_1$与$l_i$、$l_2$与$l_3$中的较大值；如果$l_2$与$l_3$重复，则继续尝试，假设最优为$l_2$与$l_j$；继续尝试$l_3$，得到最优组合，在上述三个组合中选择最大的值；问题在于当$l_1$、$l_2$、$l_3$在同一个端点重合时，是否还要继续尝试$l_4$等等呢？如图，假设这种情况$l_k$、$l_g$为最优解，$k,g \ge 4$，此时分情况如下：
  * $l_k$、$l_g$中至少一条不与$l_1$、$l_2$、$l_3$中至少一条重复，即至少一条边不与公共端点`A`相连，则$l_1$、$l_2$、$l_3$中必然存在一条不重复边与$l_k$、$l_g$中一条构成更大的组合；
  * $l_k$、$l_g$与$l_1$、$l_2$、$l_3$都重复在端点`A`，显然$l_1$与$l_2$的组合更优。
* 经过上述推论可知最优组合必然包含$l_1$、$l_2$、$l_3$中的一个或多个，这样得到优化三，即只需尝试前三大的边与后续边的组合，时间复杂度降低为$O(N3K)$。

<img src="..\images\#lcp16_2.png" style="zoom:80%;" />

* 要实现上述思路，每个顶点对应的边的集合按照权重大小逆序排列，在实现上可先将边的数组排序。

```java
class Solution {
    public int maxWeight(int[][] edges, int[] value) {
        if (edges == null || edges.length < 3) return 0;

        int M = edges.length, N = value.length;
        // 根据边的两个端点权重排序，逆序
        Arrays.sort(edges, (a, b) -> value[b[0]] + value[b[1]] - value[a[0]] - value[a[1]]);

        // 无向图出度统计
        int[] outDegree = new int[N];
        for (int[] edge : edges) {
            outDegree[edge[0]]++;
            outDegree[edge[1]]++;
        }

        // 根据出度构建有向图
        Node[] graph = new Node[N];
        for (int i = 0; i < M; i++) {
            int[] edge = edges[i];
            // 出度小的指向出度大的，如果相等则序号小的指向序号大的
            if (outDegree[edge[0]] < outDegree[edge[1]] || 
                (outDegree[edge[0]] == outDegree[edge[1]] && edge[0] < edge[1])) 
                graph[edge[0]] = new Node(edge[1], i, graph[edge[0]]);
            else graph[edge[1]] = new Node(edge[0], i, graph[edge[1]]);
        }

        // 保存边及构成三角形的端点
        List<Integer>[] edgeToVer = new ArrayList[M];
        // 假设当前为BC边，所连AC边A端点对应BC边的idx及AB边的idx
        int[] visited = new int[N], idx = new int[N];
        for (int i = 0; i < M; i++) {
            int[] edge = edges[i];

            // 端点1连接的其他端点
            for (Node temp = graph[edge[0]]; temp != null; temp = temp.next) {
                visited[temp.ver] = i + 1;
                idx[temp.ver] = temp.idx;
            }
            // 检查端点2是否与1相连的端点构成三角形
            for (Node temp = graph[edge[1]]; temp != null; temp = temp.next) {
                // 构成三角形
                if (visited[temp.ver] == i + 1) {
                    if (edgeToVer[i] == null) edgeToVer[i] = new ArrayList<>();
                    if (edgeToVer[idx[temp.ver]] == null) edgeToVer[idx[temp.ver]] = new ArrayList<>();
                    if (edgeToVer[temp.idx] == null) edgeToVer[temp.idx] = new ArrayList<>();
                    // 维护三个边及对应端点
                    edgeToVer[i].add(temp.ver); // BC对应端点A
                    edgeToVer[idx[temp.ver]].add(edge[1]); // AB对应端点C
                    edgeToVer[temp.idx].add(edge[0]); // AC对应端点B
                }
            }
        }

        // 保存端点对应边（边呈降序），边由大到小遍历
        List<Integer>[] nodes = new ArrayList[N];
        for (int i = 0; i < M; i++) {
            if (edgeToVer[i] == null) continue;
            for (int ver : edgeToVer[i]) {
                if (nodes[ver] == null) nodes[ver] = new ArrayList<>();
                nodes[ver].add(i);
            }
        }

        int max = 0;
        // 对每个端点及构成三角形的边遍历计算
        for (int i = 0; i < N; i++) {
            List<Integer> l = nodes[i];
            if (l == null || l.size() == 0) continue;
            
            // 剪枝
            int limit = l.size() - 1;
            for (int j = 0; j < Math.min(3, l.size()) && limit >= j; j++) {
                int edge1 = l.get(j), weight = getWeight(value, i, edges[edge1][0], edges[edge1][1]);
                // 注意k = j，包含了只有一个三角形的情况
                for (int k = j; k <= limit; k++) {
                    int edge2 = l.get(k), cur = weight, count = 0;
                    if (edges[edge2][0] != edges[edge1][0] && edges[edge2][0] != edges[edge1][1]) {
                        cur += value[edges[edge2][0]];
                        count++;
                    }
                    if (edges[edge2][1] != edges[edge1][0] && edges[edge2][1] != edges[edge1][1]) {
                        cur += value[edges[edge2][1]];
                        count++;
                    }
                    max = Math.max(max, cur);
                    // 存在两条边无重复点，则之后无需遍历
                    if (count == 2) {
                        limit = k - 1;
                        break;
                    }
                }
            }
        }

        return max;
    }

    private int getWeight(int[] value, int... vers) {
        int weight = 0;
        for (int ver : vers) weight += value[ver];
        return weight;
    }
}

class Node {
    int ver;
    // 边在数组中的索引
    int idx;

    Node next;

    Node(int ver, int idx, Node next) {
        this.ver = ver;
        this.idx = idx;
        this.next = next;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\max(M\sqrt{M},NK))$，其中$K$为平均每个顶点对应的三角形边数目，空间复杂度为$O(N + M)$。

执行用时：92ms，在所有java提交中击败了100.00%的用户。

内存消耗：65.1MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。