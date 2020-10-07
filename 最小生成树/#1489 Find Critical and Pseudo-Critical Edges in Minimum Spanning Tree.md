[toc]

Given a weighted undirected connected graph with $n$ vertices numbered from $0$ to $n-1$, and an array `edges` where `edges[i] = [fromi, toi, weighti]` represents a bidirectional and weighted edge between nodes `fromi` and `toi`. A minimum spanning tree (MST) is a subset of the edges of the graph that connects all vertices without cycles and with the minimum possible total edge weight.

Find all the critical and pseudo-critical edges in the minimum spanning tree (MST) of the given graph. An MST edge whose deletion from the graph would cause the MST weight to increase is called a critical edge. A pseudo-critical edge, on the other hand, is that which can appear in some MSTs but not all.

Note that you can return the indices of the edges in any order.



**Constraints**:

* $2 \le n \le 100$
* $1 \le \text{edges.length} <= \min(200, n * (n - 1) / 2)$
* $\text{edges[i].length} == 3$
* $0 \le \text{from}_i < \text{to}_i < n$
* $1 \le \text{weight}_i \le 1000$
* All pairs `(fromi, toi)` are distinct.



## 题目解读

&emsp;定义最小生成树的关键边为若从图中删除，则导致最小生成树的权值和增加，伪关键边为出现在某些最小生成树中但不会出现在所有最小生成树中。求出所有的关键边和伪关键边。

```java
class Solution {
    public List<List<Integer>> findCriticalAndPseudoCriticalEdges(int n, int[][] edges) {

    }
}
```

## 程序设计

* 由于节点较少，可采用暴力判断的方式，即首先生成最小生成树，然后遍历每条边，判断是否是关键边或伪关键边；
* 判断关键边只需在生成最小生成树时删除该边，然后生成最小生成树，如果权重和变大或不连通，则说明当前边是关键边；
* 判断伪关键边只需提前加入当前边，然后生成最小生成树，如果权重和与完整最小生成树一致，则表示是伪关键边。

```java
class Solution {
    public List<List<Integer>> findCriticalAndPseudoCriticalEdges(int n, int[][] edges) {
        List<List<Integer>> res = new LinkedList<>();
        res.add(new LinkedList<>());
        res.add(new LinkedList<>());
        // 最小生成树权重
        int weight = mst(n, edges, false, -1);
        // 不连通
        if (weight == -1) return res;

        // 删除i条边的最小生成树
        for (int i = 0; i < edges.length; i++) {
            // 关键边
            if (mst(n, edges, false, i) != weight) res.get(0).add(i);
            // 伪关键边
            else if (mst(n, edges, true, i) == weight) res.get(1).add(i);
        }
        return res;
    }

    // 加入/删除某条边生成最小生成树
    private int mst(int n, int[][] edges, boolean flag, int k) {
        int accept = 0, weight = 0;
        DisJoint disJoint = new DisJoint(n);
        // 加入边
        if (flag) {
            accept++;
            weight += edges[k][2];
            disJoint.union(edges[k][0], edges[k][1]);
        }
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> a[2] - b[2]);
        for (int i = 0; i < edges.length; i++) {
            if (i == k) continue;
            queue.add(edges[i]);
        }
        
        while (accept < n - 1 && !queue.isEmpty()) {
            int[] edge = queue.poll();
            int r1 = disJoint.find(edge[0]), r2 = disJoint.find(edge[1]);
            // 加入最小生成树
            if (r1 != r2) {
                accept++;
                weight += edge[2];
                disJoint.union(r1, r2);
            }
        }

        return disJoint.size == 1 ? weight : -1;
    }
}

class DisJoint {
    int[] parent;
    int size;

    DisJoint(int size) {
        this.size = size;
        this.parent = new int[size];
        Arrays.fill(parent, -1);
    }

    public void union(int r1, int r2) {
        if (parent[r1] >= 0 || parent[r2] >= 0) throw new IllegalArgumentException("invalid param");
        if (r1 == r2) return;
        if (parent[r1] <= parent[r2]) {
            parent[r1] += parent[r2];
            parent[r2] = r1;
        } else {
            parent[r2] += parent[r1];
            parent[r1] = r2;
        }
        size--;
    }

    public int find(int idx) {
        if (parent[idx] < 0) return idx;
        return parent[idx] = find(parent[idx]);
    }
}
```

> 可继续优化，提前排序边，不用每次最小生成树都排序，可以大大减少时间。

## 性能分析

&emsp;时间复杂度为$O(E^2\log_2E)$，空间复杂度为$O(\max(V, E))$。

执行用时：224ms，在所有java提交中击败了8.86%的用户。

内存消耗：39.4MB，在所有java提交中击败了55.77%的用户。

## 官方解题

&emsp;暂无，密切关注。上述暴力思路时间性能较差，进阶算法为`tarjan`，即在原来生成最小树算法的基础上，将同样权重的边与之前的加入最小生成树（此时可能是未连通的森林）的点构成新的图，然后通过`tarjan`查找桥，就是关键边，否则是伪关键边。有如下改动：

* 首先构图不使用原始点，而是将森林中单个树看作一个点，如`[[0,1,1],[1,2,1],[0,2,1],[2,3,4],[3,4,2],[3,5,2],[4,5,2]]`，在处理完权重为$1$的所有边后，节点`0`、`1`、`2`并入不相交集，看作是一个点，然后处理权重为$2$的边，节点`3`、`4`、`5`看作一个点，最后处理`[2,3,4]`，此时构图不能使用节点`3`、`4`，而是要使用两个不相交集的根节点，避免存在`[2,3,4]`、`[0,5,4]`等时连到不同的点；
* 其次需要注意`tarjan`判断回到父节点的问题，不同于普通`tarjan`使用节点编号判断是否回到父节点，由于将森林中单个树看作是一个点，存在多条端点一致的边，需要区分对待，如` [[0,1,1],[1,2,1],[2,3,2],[0,3,2],[0,4,3],[3,4,3],[1,4,6]]`在处理完权重$1$的边后，`0`，`1`，`2`看作一个点`x`，此时`[2,3,2]`、`[0,3,2]`实际都是`[x,3,2]`，如果不加区分，使用节点编号会造成误判，故此处区分父节点使用边的索引，这样上述两个边编号不同，`tarjan`可从`x`通过索引为$2$到边到达`3`，然后通过索引为$3$的边回到`x`，从而形成环路，这两条边都是伪关键边。
* 最后处理完当前权重的边后要从图中删除，并清除`tarjan`计数。

```java
class Solution {
    int idx = 1;

    public List<List<Integer>> findCriticalAndPseudoCriticalEdges(int n, int[][] edges) {
        int m = edges.length;
        Integer[] edgeIndex = new Integer[m];
        for (int i = 0; i < m; i++) edgeIndex[i] = i;
        // 根据边的权重排序
        Arrays.sort(edgeIndex, (a, b) -> edges[a][2] - edges[b][2]);

        // 1表示关键边，2表示伪关键边
        int[] mark = new int[m];
        // tarjan算法相关数据结构
        int[] index = new int[n];
        int[] lowlink = new int[n];

        int left = 0, right = 1;
        Node[] graph = new Node[n];
        DisJoint disJoint = new DisJoint(n);
        while (right <= m) {
            // 权重不相等，将之前相等的权重处理
            if (right == m || edges[edgeIndex[left]][2] != edges[edgeIndex[right]][2]) {
                for (int i = left; i < right; i++) {
                    int[] edge = edges[edgeIndex[i]];
                    int r1 = disJoint.find(edge[0]), r2 = disJoint.find(edge[1]);
                    // 形成环，不符合要求
                    if (r1 == r2) continue;
                    // 创建图
                    graph[r1] = new Node(r2, edgeIndex[i], edge[2], graph[r1]);
                    graph[r2] = new Node(r1, edgeIndex[i], edge[2], graph[r2]);
                    // 至少是伪关键图
                    mark[edgeIndex[i]] = 2;
                }

                // tarjan查找桥
                for (int i = left; i < right; i++) {
                    int[] edge = edges[edgeIndex[i]];
                    int r1 = disJoint.find(edge[0]), r2 = disJoint.find(edge[1]);
                    // 形成环或已经遍历
                    if (r1 == r2 || index[r1] > 0) continue;
                    tarjan(r1, -1, graph, index, lowlink, mark);
                }

                // 合并节点，删除图
                for (int i = left; i < right; i++) {
                    int[] edge = edges[edgeIndex[i]];
                    int r1 = disJoint.find(edge[0]), r2 = disJoint.find(edge[1]);
                    if (r1 == r2) continue;
                    disJoint.union(r1, r2);
                    graph[r1] = null;
                    graph[r2] = null;
                    index[r1] = index[r2] = lowlink[r1] = lowlink[r2] = 0;
                }
                // 迭代
                left = right;
            }
            right++;
        }

        List<List<Integer>> res = new LinkedList<>();
        res.add(new LinkedList<>());
        res.add(new LinkedList<>());
        for (int i = 0; i < m; i++) {
            if (mark[i] == 0) continue;
            res.get(mark[i] - 1).add(i);
        }
        return res;
    }

    // 此处pre为cur所在边的索引
    private void tarjan(int cur, int pre, Node[] graph, int[] index, int[] lowlink, int[] mark) {
        index[cur] = lowlink[cur] = idx++;

        for (Node tmp = graph[cur]; tmp != null; tmp = tmp.next) {
            // 无向图避免回去
            if (tmp.idx == pre) continue;
            // 未访问
            if (index[tmp.ver] == 0) {
                tarjan(tmp.ver, tmp.idx, graph, index, lowlink, mark);
                lowlink[cur] = Math.min(lowlink[cur], lowlink[tmp.ver]);
                // 桥，是关键边
                if (lowlink[tmp.ver] > index[cur]) {
                    mark[tmp.idx] = 1;
                }
            } else {
                lowlink[cur] = Math.min(lowlink[cur], index[tmp.ver]);
            }
        }
    }
}

class Node {
    int ver;
    int idx;
    int weight;
    Node next;

    Node(int ver, int idx, int weight, Node next) {
        this.ver = ver;
        this.idx = idx;
        this.weight = weight;
        this.next = next;
    }
}

class DisJoint {
    int[] parent;
    int size;

    DisJoint(int size) {
        this.size = size;
        this.parent = new int[size];
        Arrays.fill(parent, -1);
    }

    public void union(int r1, int r2) {
        if (parent[r1] >= 0 || parent[r2] >= 0) throw new IllegalArgumentException("invalid param");
        if (r1 == r2) return;
        if (parent[r1] <= parent[r2]) {
            parent[r1] += parent[r2];
            parent[r2] = r1;
        } else {
            parent[r2] += parent[r1];
            parent[r1] = r2;
        }
        size--;
    }

    public int find(int idx) {
        if (parent[idx] < 0) return idx;
        return parent[idx] = find(parent[idx]);
    }
}
```

&emsp;排序花费$O(E\log_2E)$，`tarjan`过程花费$O(V + E)$，总的时间复杂度为$O(\max(E\log_2E, V + E))$，空间复杂度为$O(V + E)$。

执行用时：11ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.9MB，在所有java提交中击败了96.15%的用户。