[toc]

In a group of $N$ people (labelled $0, 1, 2, ..., N-1$), each person has different amounts of money, and different levels of quietness.

For convenience, we'll call the person with label `x`, simply "person `x`".

We'll say that `richer[i] = [x, y]` if person `x` definitely has more money than person `y`.  Note that `richer` may only be a subset of valid observations.

Also, we'll say `quiet[x] = q` if person x has quietness `q`.

Now, return `answer`, where `answer[x] = y` if `y` is the least quiet person (that is, the person y with the smallest value of `quiet[y]`), among all people who definitely have equal to or more money than person `x`.

 

**Example 1**:

```
Input: richer = [[1,0],[2,1],[3,1],[3,7],[4,3],[5,3],[6,3]], quiet = [3,2,5,4,6,1,7,0]
Output: [5,5,2,5,4,5,6,7]
Explanation: 
answer[0] = 5.
Person 5 has more money than 3, which has more money than 1, which has more money than 0.
The only person who is quieter (has lower quiet[x]) is person 7, but
it isn't clear if they have more money than person 0.

answer[7] = 7.
Among all people that definitely have equal to or more money than person 7
(which could be persons 3, 4, 5, 6, or 7), the person who is the quietest (has lower quiet[x])
is person 7.

The other answers can be filled out with similar reasoning.
```


**Note**:

* $1 \le \text{quiet.length} = N \le 500$
* $0 \le \text{quiet[i]} < N$, all `quiet[i]` are different.
* $0 \le \text{richer.length} \le N * (N-1) / 2$
* $0 \le \text{richer[i][j]} < N$
* $\text{richer[i][0]} \ne \text{richer[i][1]}$
* `richer[i]`'s are all different.
* The observations in `richer` are all logically consistent.



## 题目解读

&emsp;求在所有拥有钱不少于`x`的人中最安静的人。

```java
class Solution {
    public int[] loudAndRich(int[][] richer, int[] quiet) {

    }
}
```

## 程序设计

* 由于需要直到比某个人富的所有人，可以构图，较穷的人指向较富的人；其次由于人之间的比较关系不会出现互相依赖的关系，即图是有向无环图；
* 这样就可以使用深度优先搜索遍历来更新每个人比他富有（包含本人）的所有人里最安静的人了。

```java
class Solution {
    public int[] loudAndRich(int[][] richer, int[] quiet) {
        int n = quiet.length;
        // 构图
        Node[] graph = new Node[n];
        for (int[] relation : richer) {
            int rich = relation[0], poor = relation[1];
            // 穷人指向富人
            graph[poor] = new Node(rich, graph[poor]);
        }

        int[] res = new int[n];
        Arrays.fill(res, -1);

        for (int ver = 0; ver < n; ver++) {
            if (res[ver] != -1) continue;
            dfs(ver, graph, quiet, res);
        }
        return res;
    }

    private int dfs(int ver, Node[] graph, int[] quiet, int[] res) {
        if (res[ver] != -1) return res[ver];
        // 子树最小的编号（初始化为自身）
        int idx = ver;
        for (Node tmp = graph[ver]; tmp != null; tmp = tmp.next) {
            int minChild = dfs(tmp.ver, graph, quiet, res);
            if (quiet[minChild] < quiet[idx]) idx = minChild;
        }
        return res[ver] = idx;
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

&emsp;时间复杂度为$O(EV)$，空间复杂度为$O(EV)$。

执行用时：5 ms, 在所有 Java 提交中击败了99.31%的用户

内存消耗：47.4 MB, 在所有 Java 提交中击败了42.86%的用户。

## 官方解题

&emsp;官方思路类似。