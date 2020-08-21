[toc]

&emsp;图的基本概念包括图的存储和搜索。图的存储使用邻接表或邻接矩阵，拓展的有逆邻接表、十字链表、邻接多重表；图的搜索主要分为深度优先搜索和广度优先搜索。一下伪代码示例中`Node[] graph`表示图的邻接表表示，`int[][] graph`为邻接矩阵表示。链表结构通用如下：

```java
class Node {
    // 结点编号
    int ver;
    // 后继
    Node next;
}
```

## 图的搜索

&emsp;最基本的，图的遍历需要记录结点是否访问过，`dfs`伪代码实现如下：

```java
// 对于图的各个连通分量进行遍历
for (int i = 0; i < N; i++) {
    if(!visited[i]) dfs(i, graph, visited);
}
```

深度优先搜索递归代码如下：

```java
private void dfs(int start, Node[] graph, boolean[] visited) {
    // 已访问，不重复访问
    if (visited[start]) return;
    // 标记为已访问
    visited[start] = true;
    // 遍历后继结点
    Node next = graph[start];
    while (next != null) {
        dfs(next.ver, graph, visited);
        next = next.next;
    }
}
```

`bfs`伪代码如下：

```java
// 对于图的各个连通分量进行遍历
Queue<Integer> queue = new LinkedList<>();
for (int i = 0; i < N; i++) {
    if(visited[i]) continue;
    
    // 对当前连通分量进行遍历
    queue.add(i);
    while (!queue.isEmpty()) {
        int cur = queue.poll();
        visited[cur] = true;
         // 遍历后继结点
    	Node next = graph[start];
    	while (next != null) {
            // 将未访问的结点加入队列
        	if (!visited[next.ver]) queue.add(next.ver);
        	next = next.next;
   	 	}
    }
}
```

&emsp;图的遍历有很多变体应用，如着色问题，图中是否存在路径等，这些都只是在上述基本遍历结构基础上做改动；如访问标识上面是布尔型，只表示是否访问过，完全可以变为整形，定义更多的状态，如正在遍历、已遍历无环、已遍历有环、未遍历等；除了访问标识，递归函数返回值也是一个改动点，可以返回布尔型，如果图不满足某些条件，提前返回，方便判断，也可以返回其它类型，如后继结点的颜色等。

* 中等[#133 Clone Graph](./#133 Clone Graph.md)    图的遍历拷贝
* $\clubs$困难[#631 Design Excel Sum Formula](./#631 Design Excel Sum Formula.md)    图的深度优先搜索的应用
* $\clubs$中等[#785 Is Graph Bipartite?](./#785 Is Graph Bipartite.md)    图遍历的变种，巧妙利用访问标识
* $\clubs$中等[#802 Find Eventual Safe States](./#802 Find Eventual Safe States.md)    图遍历的变种，巧妙利用访问标识
* 简单[#841 Keys and Rooms](./#841 Keys and Rooms.md)    图的遍历
* 中等[#797 All Paths From Source to Target](./#797 All Paths From Source to Target.md)    深度优先搜索遍历路径
* 中等[#1059 All Paths from Source Lead to Destination](./#1059 All Paths from Source Lead to Destination.md)    图遍历的变种，巧妙利用访问标识
* $\clubs$中等[#1042 Flower Planting With No Adjacent](./#1042 Flower Planting With No Adjacent.md)    深度遍历变形着色结点
* 中等[#1236 Web Crawler](./#1236 Web Crawler.md)    从一个节点开始的图的遍历

## 图的搜索在其它领域的应用

* $\clubs$困难[#126 Word Ladder II](./#126 Word Ladder II.md)    [#127 Word Ladder](./#127 Word Ladder.md)的进阶
* $\bigstar$中等[#127 Word Ladder](./#127 Word Ladder.md)    最短的词语接龙
* 中等[#286 Walls and Gates](./#286 Walls and Gates.md)    矩形的广度优先搜索
* $\clubs$困难[#317 Shortest Distance from All Buildings](./#317 Shortest Distance from All Buildings.md)    从建筑开始广度优先遍历
* $\bigstar$中等[#329 Longest Increasing Path in a Matrix](./#329 Longest Increasing Path in a Matrix.md)    深度优先搜索及拓扑排序的应用
* 中等[#417 Pacific Atlantic Water Flow](./#417 Pacific Atlantic Water Flow.md)    两次广度优先搜索
* 困难[#489 Robot Room Cleaner](./#489 Robot Room Cleaner.md)    未知图的分布情况下的深度优先搜索
* 中等[#490 The Maze](./#490 The Maze.md)    拐点只能访问一次
* 困难[#499 The Maze III](./#499 The Maze III.md)    [#505 The Maze II](./#505 The Maze II.md)的进阶
* $\clubs$中等[#505 The Maze II](./#505 The Maze II.md)    [#490 The Maze](./#490 The Maze.md)进阶
* 中等[#529 Minesweeper](./#529 Minesweeper.md)    图的深度优先搜索在扫雷中的应用
* 中等[#542 01 Matrix](./#542 01 Matrix.md)    广度优先搜索计算最短距离
* $\clubs$困难[#675 Cut Off Trees for Golf Event](./#675 Cut Off Trees for Golf Event.md)    广度优先搜索计算两点间最短距离
* 中等[#694 Number of Distinct Islands](./#694 Number of Distinct Islands.md)    深度优先搜索路径
* 中等[#695 Max Area of Island](./#695 Max Area of Island.md)    矩形的深度优先搜索
* $\clubs$困难[#711 Number of Distinct Islands II](./#711 Number of Distinct Islands II.md)    [#694 Number of Distinct Islands](./#694 Number of Distinct Islands.md)的进阶
* 简单[#733 Flood Fill](./#733 Flood Fill.md)    深度优先搜索渲染像素
* $\clubs$中等[#752 Open the Lock](./#752 Open the Lock.md)    双向广度优先遍历
* $\clubs$困难[#773 Sliding Puzzle](./#773 Sliding Puzzle.md)    滑板游戏抽象为广度优先搜索遍历
* $\bigstar$困难[#847 Shortest Path Visiting All Nodes](./#847 Shortest Path Visiting All Nodes.md)    无向连通图遍历所有顶点的最短路径
* $\bigstar$困难[#854 K-Similar Strings](./#854 K-Similar Strings.md)    将广度优先搜索应用到字符串的生成
* $\clubs$中等[#909 Snakes and Ladders](./#909 Snakes and Ladders.md)    蛇梯棋广度优先搜索
* $\clubs$中等[#934 Shortest Bridge](./#934 Shortest Bridge.md)    深度、广度优先搜索结合的贪心策略
* 中等[#994 Rotting Oranges](./#994 Rotting Oranges.md)    广度优先搜索
* 简单[#1030 Matrix Cells in Distance Order](./#1030 Matrix Cells in Distance Order.md)    图的广度优先搜索思想在矩形中的应用
* $\clubs$繁杂[#1036 Escape a Large Maze](./#1036 Escape a Large Maze.md)    采用有限制步数的双向广度、深度搜索确定是否存在包围圈
* 中等[#1091 Shortest Path in Binary Matrix](./#1091 Shortest Path in Binary Matrix.md)    矩阵的广度优先搜索
* $\clubs$中等[#1162 As Far from Land as Possible](./#1162 As Far from Land as Possible.md)    广度优先搜索从外层向中心遍历
* $\clubs$中等[#1197 Minimum Knight Moves](./#1197 Minimum Knight Moves.md)    快速逼近加剪枝的广度优先搜索
* $\clubs$困难[#1293 Shortest Path in a Grid with Obstacles Elimination](./#1293 Shortest Path in a Grid with Obstacles Elimination)    拓展的广度优先搜索
* 简单[#1306 Jump Game III](./#1306 Jump Game III.md)    图的遍历在数组的应用
* 困难[#1345 Jump Game IV](./#1345 Jump Game IV.md)    广度优先搜索前数据处理

## 无向图的连通性

&emsp;无向图的连通性比较简单，最基础的可以使用不相交集，采用`dfs`和`bfs`也可以，依据就是上面伪代码中的：

```java
for (int i = 0; i < N; i++) {
    if(!visited[i]) 深度优先搜索/广度优先搜索;
}
```

连通分量就是最外层循环里符合`if`要求的数目，可以加一个计数统计即可。相比来说不相交集的做法更优雅，因为省去了由边构建图的过程（当然如果图直接给出的话，性能都一样）。

&emsp;除了整个图的连通性，还有从一个结点出发的连通性，仍然是上述思路，不相交集最后统计集合数目是否是1，而`dfs`和`bfs`去除最外层循环，从指定起点出发，最后遍历访问标识数组，查看是否全部遍历完即可。

## 无向图欧拉回路

&emsp;首先需要直到欧拉回路存在的满足条件，即每个结点的度为偶数，欧拉路径满足条件为起始点和结束点度为奇数，其它点度为偶数。查找欧拉回路的基本思想是环路拼接，与点的遍历不同，欧拉回路是边的遍历，每遍历完一条边，就删除这条边，如果一次遍历结束回到了起始点，存在边没遍历完，则继续遍历拼接。欧拉回路涉及的算法题较少，无向图的欧拉回路知识可以应用到有向图的欧拉回路中，此时对于欧拉回路，每个结点的入度和出度相同；对于欧拉路径，起始点出度比入度多一，结束点少一，其它点入度、出度相同。

* $\bigstar$困难[#332 Reconstruct Itinerary](./#332 Reconstruct Itinerary.md)    有向图欧拉路径或回路的查找
* $\bigstar$技巧[#753 Cracking the Safe](./#753 Cracking the Safe.md)    有向图欧拉回路问题

## 有向图的连通性

&emsp;有向图的连通性一般考察比较基础的知识，如果从一个点到另一个点是否连通，使用`dfs`和`bfs`即可。事实上有向图的连通性主要体现在连通图的强连通分量上。如何查找强连通分量是关键，对于一个有向图，首先深度优先搜索，对结点按照访问顺序标记编号，然后将图的边反向，从编号大的点开始遍历，得到的就是强连通森林。这就是`Kosaraju`算法。`Tarjan`算法是`Kosaraju`算法的改进，给每个节点一个深度优先搜索标号`index`，表示是第几个被访问的节点。此外每个节点还有一个值`lowlink`，表示从该节点出发经有向边可到达的所有节点中最小的`index`。显然`lowlink`总是不大于`index`，且当从节点出发经有向边不能到达其他节点时这两个值相等。节点是强连通分量的根当且仅当`lowlink = index`。

* $\bigstar$困难[#1192 Critical Connections in a Network](./#1192 Critical Connections in a Network.md)    `Tarjan`算法在无向图桥中的应用
* 中等[#1361 Validate Binary Tree Nodes](./#1361 Validate Binary Tree Nodes.md)    图的特例树的图像特征

## 有向无环图拓扑排序

&emsp;有向图的拓扑排序用于依赖关系的分析排序，根据思想不同分两种实现方式：入度优先思想，即入度为0的点是依赖性最高的点，采用广度优先搜索来实现；出度优先思想，即出度为0的点是依赖性最低的点，采用深度优先搜索实现。广度优先的实现需要统计入度，每次将入度为0的点加入队列；深度优先的实现基本形式与`dfs`一致，只是遍历完后继结点，需要记录当前结点到序列。拓扑排序需要根据题意具体分析关系，一般都是相同的问题，小的改动，拓扑排序衍生问题比如[#310 Minimum Height Trees](./#310 Minimum Height Trees.md)，采用从图的边缘结点层层向中心逼近，[#1203 Sort Items by Groups Respecting Dependencies](./#1203 Sort Items by Groups Respecting Dependencies.md)父子图拓扑排序，需要关注了解。

* $\clubs$中等[#210 Course Schedule II](./#210 Course Schedule II.md)    [#207 Course Schedule](./#207 Course Schedule.md)的延续，有向无环图拓扑排序
* 中等[#269 Alien Dictionary](./#269 Alien Dictionary.md)    拓扑排序
* $\bigstar$中等[#310 Minimum Height Trees](./#310 Minimum Height Trees.md)    拓扑排序思想的扩展
* 繁杂[#444 Sequence Reconstruction](./#444 Sequence Reconstruction.md)    拓扑排序变种应用
* 简单[#997 Find the Town Judge](./#997 Find the Town Judge.md)    出度、入度的简单应用
* 中等[#1136 Parallel Courses](./#1136 Parallel Courses.md)    拓扑排序求层数
* $\clubs$繁杂[#1203 Sort Items by Groups Respecting Dependencies](./#1203 Sort Items by Groups Respecting Dependencies.md)    父子图的拓扑排序
* 中等[#1273 Delete Tree Nodes](./#1273 Delete Tree Nodes.md)    拓扑排序思想后序遍历树的父子节点表示

## 有向图环路判断

&emsp;有向图是否存在环的判断是图遍历的改进，可使用`dfs`、`bfs`实现，也可以使用拓扑排序实现。`dfs`实现需要改进访问数组，增加标识状态；拓扑排序的实现无需改动结构，只需最后判断是否所有的结点入度为0即可。

* $\bigstar$中等[#207 Course Schedule](./#207 Course Schedule.md)    深度优先、广度优先在环路判断中的应用
* $\clubs$技巧[#1153 String Transforms Into Another String](./#1153 String Transforms Into Another String.md)    将字符串转换使用图来理解

## 二分图最大匹配问题

* $\bigstar$困难[#LCP04 Broken Board Dominoes](./#LCP04 Broken Board Dominoes.md)    匈牙利算法