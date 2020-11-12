[toc]

You have $N$ gardens, labelled $1$ to $N$. In each garden, you want to plant one of 4 types of flowers.

`paths[i] = [x, y]` describes the existence of a bidirectional path from garden `x` to garden `y`.

Also, there is no garden that has more than 3 paths coming into or leaving it.

Your task is to choose a flower type for each garden such that, for any two gardens connected by a path, they have different types of flowers.

Return **any** such a choice as an array `answer`, where `answer[i]` is the type of flower planted in the `(i+1)`-th garden. The flower types are denoted 1, 2, 3, or 4. It is guaranteed an answer exists.



**Note:**

- $1 \le N \le 10000$
- $0 \le \text{paths.size} \le 20000$
- No garden has 4 or more paths coming into or leaving it.
- It is guaranteed an answer exists.



## 题目解读

&emsp;有$N$个花园，每个花园可以种四种颜色中的一种花。题目需要给出任意一种种花方案，使得相邻的两个花园花不一样。题目限定每个点最多有三个边，且肯定存在答案。

```java
class Solution {
    public int[] gardenNoAdj(int N, int[][] paths) {
        
    }
}
```

## 程序设计

* 首先想到的是自顶向下，但是这样存在的问题是上层结点着色后需要传递着色信息给下层结点，但问题是存在多个上层结点的下层结点很难同时判断上层结点的着色。
* 如果自底向上，深度优先搜索，则上层结点可以容易的得到所有的下层结点的着色。

```java
class Solution {
    public int[] gardenNoAdj(int N, int[][] paths) {
        if (N < 1 || paths == null) throw new IllegalArgumentException("invalid param");
        // 构建图
        Node[] graph = new Node[N];
        for (int[] path : paths) {
            int x = path[0] - 1, y = path[1] - 1;
            graph[x] = new Node(y, graph[x]);
            graph[y] = new Node(x, graph[y]);
        }
        // 深度优先搜索
        int[] color = new int[N];
        for (int i = 0; i < N; i++) {
            if (color[i] != 0) continue;
            dfs(i, graph, color);
        }
        return color;
    }

    private int dfs(int start, Node[] graph, int[] color) {
        // 当前结点存在环，则在当前结点随意设置一个颜色并返回
        if (color[start] == -1) color[start] = 1;
        // 当前结点已有颜色，直接返回
        if (color[start] != 0) return color[start];
        
        // 标记为正在遍历
        color[start] = -1;
        Node temp = graph[start];
        // 标记可用颜色
        boolean[] used = new boolean[4];
        while (temp != null) {
            // 将后继节点的着色标记为不可用，注意索引越界，需要减一
            used[dfs(temp.ver, graph, color) - 1] = true;
            temp = temp.next;
        }
        for (int i = 0; i < 4; i++) {
            // 找到可用颜色
            if (!used[i]) {
                color[start] = i + 1;
                break;
            }
        }
        return color[start];
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

&emsp;时间复杂度为$O(V + E)$，空间复杂度为$O(V + E)$。

执行用时：9ms，在所有java提交中击败了80.05%的用户。

内存消耗：48MB，在所有java提交中击败了80.33%的用户。

## 官方解题

&emsp;暂无，密切关注。