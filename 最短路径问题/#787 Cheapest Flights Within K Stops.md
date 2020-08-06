[toc]

There are $n$ cities connected by $m$ flights. Each flight starts from city $u$ and arrives at $v$ with a price $w$.

Now given all the cities and flights, together with starting city `src` and the destination `dst`, your task is to find the cheapest price from `src` to `dst` with up to $k$ stops. If there is no such route, output $-1$.



**Constraints**:

* The number of nodes $n$ will be in range $[1, 100]$, with nodes labeled from $0$ to $n - 1$.
* The size of `flights` will be in range $[0, n * (n - 1) / 2]$.
* The format of each flight will be (`src`, `dst`, `price`).
* The price of each flight will be in the range $[1, 10000]$.
* $k$ is in the range of $[0, n - 1]$.
* There will not be any duplicated flights or self cycles.



## 题目解读

&emsp;给定航路图，无重复边和环，求到达目的地的最短路径，需注意题目限定最短路径上的中继点不超过$k$个。

```java
class Solution {
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int K) {
        
    }
}
```

## 程序设计

* 如果不考虑限定条件，则该问题就是普通的有向无环图最短路径问题，使用`Dijkstra`算法即可；
* 考虑限定条件，此时原`Dijkstra`算法不再适用，因为更新一个点时不再根据距离长短，还要判断路径上的节点值，有可能最短的路径节点值超过$k$个，而较长的路径符合要求；
* 根据上述分析可知问题是负权无环图最短路径问题的变形。本题的特殊之处在于需要记录节点不同$k$的路径最短值，使用队列（由于此处是目的地的最短路径，可使用优先级队列加速）贪婪遍历迭代。

```java
class Solution {
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int K) {
        if (flights == null || flights.length == 0) return -1;
        // 构图
        Node[] graph = new Node[n];
        for (int[] edge : flights) {
            int from = edge[0], to = edge[1], weight = edge[2];
            graph[from] = new Node(to, weight, graph[from]);
        }

        // 记录不同k的最短路径
        int[][] distance = new int[n][K + 1];
        for (int[] dis : distance) Arrays.fill(dis, Integer.MAX_VALUE);

        // 存储到达节点及距离、中继节点数
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> a[1] - b[1]);
        
        distance[src][0] = 0;
        queue.add(new int[]{src, 0, -1});
        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            if (cur[0] == dst) return cur[1];
            if (cur[2] >= K) continue;
            
            for (Node tmp = graph[cur[0]]; tmp != null; tmp = tmp.next) {
                int newDis = cur[1] + tmp.weight, newCount = cur[2] + 1;
                if (distance[tmp.ver][newCount] > newDis) {
                    distance[tmp.ver][newCount] = newDis;
                    queue.add(new int[]{tmp.ver, newDis, newCount});
                }
            }
        }
        
        return -1;
    }
}

class Node {
    int ver;
    int weight;

    Node next;

    Node(int ver, int weight, Node next) {
        this.ver = ver;
        this.weight = weight;
        this.next = next;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(V\log_2E)$，空间复杂度为$O(\max(V + E, KV))$。

执行用时：5ms，在所有java提交中击败了95.94%的用户。

内存消耗：39.7MB，在所有java提交中击败了97.14%的用户。

## 官方解题

&emsp;官方思路类似，使用字典记录距离。