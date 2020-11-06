[toc]

Given a set of `N` people (numbered $1, 2, \cdots, N$), we would like to split everyone into two groups of **any** size.

Each person may dislike some other people, and they should not go into the same group. 

Formally, if `dislikes[i] = [a, b]`, it means it is not allowed to put the people numbered `a` and `b` into the same group.

Return `true` if and only if it is possible to split everyone into two groups in this way.



**Constraints**:

* $1 \le N \le 2000$
* $0 \le \text{dislikes.length} \le 10000$
* $\text{dislikes[i].length} == 2$
* $1 \le \text{dislikes[i][j]} \le N$
* $\text{dislikes[i][0]} < \text{dislikes[i][1]}$
* There does not exist $i \ne j$ for which $\text{dislikes[i]} == \text{dislikes[j]}$.



## 题目解读

&emsp;判断是否可将人群根据喜好分为两部分。

```java
class Solution {
    public boolean possibleBipartition(int N, int[][] dislikes) {

    }
}
```

## 程序设计

* 题目第一印象似乎是图是否存在环的问题，可用不相交集解决，但是仔细分析，对于类似$A \to B \to C \to D \to A$的环，可以分为`A`、`C`一组，`B`、`D`一组，故不是环的判断问题。
* 可用前$N$个表示喜欢的集合，后$N$个表示不喜欢的集合，首先两个不喜欢的人不能在喜欢的集合里，然后维护新的喜欢集合。

```java
class Solution {
    public boolean possibleBipartition(int N, int[][] dislikes) {
        if (dislikes == null || dislikes.length == 0) return true;

        // 前n个为每个节点可以分为一组的集合，后n个为每个节点讨厌的一组集合
        // 后n个节点最为中间关联串联起喜欢的点的集合
        DisJoint disJoint = new DisJoint(2 * N);
        for (int[] dislike : dislikes) {
            int v1 = dislike[0] - 1, v2 = dislike[1] - 1;
            int root1 = disJoint.find(v1), root2 = disJoint.find(v2);
            if (root1 == root2) return false;

            // 不喜欢集合
            int dislike1 = disJoint.find(v1 + N), dislike2 = disJoint.find(v2 + N);

            // 维护新的喜欢集合
            disJoint.union(root1, dislike2);
            disJoint.union(root2, dislike1);
        }
        return true;
    }
}

class DisJoint {
    int[] parent;

    DisJoint(int size) {
        this.parent = new int[size];
        Arrays.fill(parent, -1);
    }

    public void union(int root1, int root2) {
        if (parent[root1] >= 0 || parent[root2] >= 0) throw new IllegalArgumentException("invalid param");

        if (root1 == root2) return;
        if (parent[root1] <= parent[root2]) {
            parent[root1] += parent[root2];
            parent[root2] = root1;
        } else {
            parent[root2] += parent[root1];
            parent[root1] = root2;
        }
    }

    public int find(int idx) {
        if (parent[idx] < 0) return idx;
        return parent[idx] = find(parent[idx]);
    }
}
```

* 也可构建图，然后通过染色来判断。该思路巧妙利用题目中$\text{dislikes[i][0]} < \text{dislikes[i][1]}$，由于是无向图，对于已染色的节点不再遍历，因为其后索引小于当前索引的节点必然已染色，大于当前节点的索引则可在之后的遍历中染色；其次对于未染色的节点统一染为红色，因为在之前的深度遍历染色时，如果当前节点连通之前节点，必然会被染色，未被染色说明当前节点不连通，起始染色并不重要，可统一为红色。

```java
class Solution {
    public boolean possibleBipartition(int N, int[][] dislikes) {
        if (dislikes == null || dislikes.length == 0) return true;

        // 构图
        Node[] graph = new Node[N];
        for (int[] dislike : dislikes) {
            int v1 = dislike[0] - 1, v2 = dislike[1] - 1;
            graph[v1] = new Node(v2, graph[v1]);
            graph[v2] = new Node(v1, graph[v2]);
        }

        // 记录颜色
        Map<Integer, Integer> color = new HashMap<>();
        for (int ver = 0; ver < N; ver++) {
            if (color.containsKey(ver)) continue;
            if (!dfs(ver, 0, graph, color)) return false;
        }
        return true;
    }

    private boolean dfs(int ver, int c, Node[] graph, Map<Integer, Integer> color) {
        // 已着色，则判断是否一致
        if (color.containsKey(ver)) return color.get(ver) == c;

        color.put(ver, c);
        for (Node temp = graph[ver]; temp != null; temp = temp.next) {
            if (!dfs(temp.ver, c ^ 1, graph, color)) return false;
        }
        return true;
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

&emsp;不相交集时间复杂度为$O(M)$，空间复杂度为$O(N)$。

执行用时：12ms，在所有java提交中击败了91.52%的用户。

内存消耗：48.3MB，在所有java提交中击败了100.00%的用户。

&emsp;构建图的方式时间复杂度为$O(M + N)$，空间复杂度为$O(M + N)$。

执行用时：15ms，在所有java提交中击败了87.50%的用户。

内存消耗：47.8MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。