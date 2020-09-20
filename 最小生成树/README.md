[toc]

## 最小生成树

&emsp;最小生成树主要有基于边的角度的`Kruskal`算法，主要用于无向图，采用不相交集和优先级队列，每次选取剩余最小的边，判断是否形成回路。伪代码如下：

```java
int accept = 0;
// 选择n - 1条边
while (!queue.isEmpty() && accept < n - 1) {
    int[] edge = queue.poll();
    if (disJoint.find(edge[0]) != disJoint.find(edge[1])) {
        disJoint.union(disJoint.find(edge[0]), disJoint.find(edge[1]));
        accept++;
    }
}
// 若连通存在最小树，则accept == n - 1
```

&emsp;从顶点的角度出发得到的是`Prim`算法。初始点集为空，每个结点的最短边数组除了起始点都为无穷大，同时需要记录是否加入最短生成树的标识数组和最短边的另一端点数组；每次循环选择最短边，并迭代下一轮端点。

```java
// 加入标识
boolean[] flag = new boolean[n];
// 当前结点路径权重，除了起始点需初始化为无穷大
int[] weight = new int[n];
// 记录上述每个结点权重对应的边的起始点（startNode为加入生成树的结点）
int[] startNode = new int[n];

// 设置开始位置
start = 起始点;
for (int i = 1; i < n; i++) {
    // 更新当前结点起始的边的权重
    for (Node temp = graph[start]; temp != null; temp = temp.next) {
        // 未加入最短生成树且权重更小
        if (!flag[temp.ver] && temp.weight < weight[temp.ver]) {
            weight[temp.ver] = temp.weight;
            startNode[temp.ver] = start;
        }
    }
    // 标记
    flag[start] = true;
    // 从当前权重选择最小值，并加入点
    for (int i = 0; i < n; i++) {
        记录最小权重min和下次遍历的起始点start
    }
    weight[start] = 无穷大
}
```

* $\clubs$中等[#1135 Connecting Cities With Minimum Cost](./#1135 Connecting Cities With Minimum Cost.md)    最小生成树基本问题
* $\bigstar$困难[#1168 Optimize Water Distribution in a Village](./#1168 Optimize Water Distribution in a Village.md)    最小生成树变形问题
* 中等[#1584 Min Cost to Connect All Points](./#1584 Min Cost to Connect All Points.md)    最小生成树的变形