[toc]

小朋友A在和他的小伙伴们玩传信息游戏，游戏规则如下：

* 有 $n$ 名玩家，所有玩家编号分别为 $0 \sim n-1$，其中小朋友 A 的编号为 $0$
* 每个玩家都有固定的若干个可传信息的其他玩家（也可能没有）。传信息的关系是单向的（比如 A 可以向 B 传信息，但 B 不能向 A 传信息）。
* 每轮信息必须需要传递给另一个人，且信息可重复经过同一个人

给定总玩家数 $n$，以及按 `[玩家编号,对应可传递玩家编号]` 关系组成的二维数组 `relation`。返回信息从小 A (编号 $0$ ) 经过 $k$ 轮传递到编号为 $n-1$ 的小伙伴处的方案数；若不能到达，返回 $0$。



**限制**：

* $2 \le n \le 10$
* $1 \le k \le 5$
* $1 \le \text{relation.length} \le 90$, 且 $\text{relation[i].length} == 2$
* $0 \le \text{relation[i][0],relation[i][1]} < n$ 且 $\text{relation[i][0]} \ne \text{relation[i][1]}$



## 题目解读

&emsp;给定图，求经过$k$个边能否到达指定点。

```java
class Solution {
    public int numWays(int n, int[][] relation, int k) {

    }
}
```

## 程序设计

* 最基本的思路是构图，然后模拟。

```java
class Solution {
    public int numWays(int n, int[][] relation, int k) {
        // 构图
        Node[] graph = new Node[n];
        for (int[] edge : relation) {
            graph[edge[0]] = new Node(edge[1], graph[edge[0]]);
        }
        // 模拟
        Queue<Integer> queue = new LinkedList<>();
        queue.add(0);
        while (k > 0 && !queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int cur = queue.poll();
                for (Node tmp = graph[cur]; tmp != null; tmp = tmp.next) {
                    queue.add(tmp.ver);
                }
            }
            k--;
        }

        int res = 0;
        // 经过k条边到达的所有的点
        while (!queue.isEmpty()) {
            if (queue.poll() == n - 1) res++;
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

&emsp;时间复杂度最坏为$O(\frac{N(1 - N^K)}{1 - N})$，空间复杂度为$O(N^K)$。

执行用时：6ms，在所有java提交中击败了19.59%的用户。

内存消耗：39.1MB，在所有java提交中击败了6.79%的用户。

## 官方解题

&emsp;官方还采用动态规划的思路，`dp(i,j)`表示$i$时刻在$j$点的数目。

```java
class Solution {
    public int numWays(int n, int[][] relation, int k) {
        int[][] dp = new int[k + 1][n];
        dp[0][0] = 1;
        for (int i = 1; i <= k; i++) {
            for (int[] edge : relation) {
                dp[i][edge[1]] += dp[i - 1][edge[0]];
            }
        }

        return dp[k][n - 1];
    }
}
```

&emsp;时间复杂度为$O(KN)$，空间复杂度为$O(KN)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.3MB，在所有java提交中击败了73.96%的用户。