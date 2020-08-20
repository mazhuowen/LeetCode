[toc]

## 单源最短路径

### 非加权图最短路径

&emsp;非加权单源路径采用广度优先搜索即可解决，需记录当前结点的距离和当前路径的前一结点。

### 加权图最短路径

&emsp;加权图最短路径采用`Dijkstra`算法，本质是贪婪法，与非加权图不同，距离需要多次更新，故还需要数组记录顶点是否加入最短路径。每次寻找当前的最短路径结点，然后更新其后继结点的距离，如此反复。

```java
// 加入最短距离标识
boolean[] known = new boolean[n];
int[] distance = new int[n];
int[] prev = new int[n];

// 初始化
distance[start] = 0;
prev[start] = start;

// 循环n-1次，因为路径最长为n-1
for (int i = 0; i < n; i++) {
    寻找未加入最短路径中距离最小的顶点minNode和minDis
    // 加入路径
    known[minNode] = true;
    // 更新后继结点
    for (Node temp = graph[minNode]; temp != null; temp = temp.next) {
        if (!known[temp.ver] && distance[temp.ver] > minDis + temp.weight) {
            distance[temp.ver] = minDis + temp.weight;
            prev[temp.ver] = minNode;
        }
    }
}
```

* $\clubs$中等[#743 Network Delay Time](./#743 Network Delay Time.md)    经典`Dijkstra`算法应用

### 负权无环图最短路径

&emsp;带有负权值的无环图需要枚举没有可能的结果，每次某个顶点找到一条更好的路径，要检查所有后继顶点，重复直到找不到更好的路径。

```java
// 初始距离为无穷大
int[] distance = new int[n];
int[] prev = new int[n];
Queue<Integer> queue = new LinkedList<>();

// 初始化
distance[start] = 0;
prev[start] = start;
queue.add(start);

while (!queue.isEmpty()) {
    int cur = queue.poll();
    // 更新所有后继结点，将更新后的结点加入队列
    for (Node temp = graph[cur]; temp != null; temp = temp.next) {
        if (distance[temp.ver] > distance[cur] + temp.weight) {
            distance[temp.ver] = distance[cur] + temp.weight;
            prev[temp.ver] = cur;
            queue.add(temp.verNum);
        }
    }
}
```

* $\clubs$中等[#787 Cheapest Flights Within K Stops](.\#787 Cheapest Flights Within K Stops.md)    限定路径节点数目的最短距离，负权问题变形
* $\clubs$中等[#1129 Shortest Path with Alternating Colors](./#1129 Shortest Path with Alternating Colors.md)    负权无环图穷举法思想的应用

### 有向无环图最短路径

&emsp;有向无环图存在拓扑排序，且起始点是入度为0的点，可以进一步简化最短路径问题。在无环图中可以按照拓扑排序的次序选择和更新顶点距离。在拓扑排序的基础上每次遍历比较后继顶点并做更新。

## 顶点对最短路径

&emsp;`Floyd`算法用于计算整个图的最短距离，其伪代码如下：

```java
// 初始化为边长
int[][] distance = new int[n][n];
// 如果不存在边初始化为-1，存在则前驱设置为边的起始顶点
int[][] prev = new int[n][n];

for (int k = 0; k < n; k++) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (distance[i][j] > diatance[i][k] + distance[k][j]) {
                distance[i][j] = diatance[i][k] + distance[k][j];
                prev[i][j] = prev[k][j];
            }
        }
    }
}
```

* $\clubs$中等[#1334 Find the City With the Smallest Number of Neighbors at a Threshold Distance](./#1334 Find the City With the Smallest Number of Neighbors at a Threshold Distance.md)    经典`Floyd`算法应用
* $\bigstar$中等[#1462 Course Schedule IV](./#1462 Course Schedule IV.md)    `Floyd`算法的顶点对可达应用