[toc]

You are given an array `points` representing integer coordinates of some points on a 2D-plane, where $\text{points[i]} = [x_i, y_i]$.

The cost of connecting two points $[x_i, y_i]$ and $[x_j, y_j]$ is the **manhattan distance** between them: $\lvert x_i - x_j\rvert + \lvert y_i - y_j\rvert$, where $\lvert val\rvert$ denotes the absolute value of `val`.

Return the minimum cost to make all points connected. All points are connected if there is **exactly one** simple path between any two points.

 

**Constraints**:

* $1 \le \text{points.length} \le 1000$
* $-10^6 \le x_i, y_i \le 10^6$
* All pairs $(x_i, y_i)$ are distinct.



## 题目解读

&emsp;给定二维平面点，求连接所有点的最短曼哈顿距离。

```java
class Solution {
    public int minCostConnectPoints(int[][] points) {

    }
}
```

## 程序设计

* 实质是最小树的问题，本题中每个点都可以连接，可以计算所有点之间的距离并入队，即`Kruskal`算法进行计算，则总的边为$\frac{n(n-1)}{2}$，时间复杂度为$O((n-1)\log_2\frac{n(n-1)}{2})$。事实上计算所有距离太浪费，由于是无向图且连通，可从点$0$开始，计算点$0$的距离，然后入堆扩展。

```java
class Solution {
    public int minCostConnectPoints(int[][] points) {
        int res = 0;
        int n = points.length;
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> a[2] - b[2]);
        DisJoint disJoint = new DisJoint(n);
        // 计算点0的边
        int start = 0;
        for (int i = 1; i < n; i++) {
            int dis = Math.abs(points[i][0] - points[start][0]) + Math.abs(points[i][1] - points[start][1]);
            queue.add(new int[]{start, i, dis});
        }
        
        // Kruskal算法
        int accept = 0;
        while (accept < n - 1 && !queue.isEmpty()) {
            int[] cur = queue.poll();
            int r1 = disJoint.find(cur[0]), r2 = disJoint.find(cur[1]);
            // 丢弃
            if (r1 == r2) continue;
            // 加入
            else {
                disJoint.union(r1, r2);
                res += cur[2];
                accept++;
                // 继续在该点扩展
                for (int i = 0; i < n; i++) {
                    // 已加入不再计算
                    if (i == cur[1] || disJoint.find(i) == disJoint.find(cur[1])) continue;
                    int dis = Math.abs(points[i][0] - points[cur[1]][0]) + Math.abs(points[i][1] - points[cur[1]][1]);
                    queue.add(new int[]{cur[1], i, dis});
                }
            }
        }
        return res;
    }
    
}

class DisJoint {
    int[] parent;
    
    DisJoint(int size) {
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
    }
    
    public int find(int idx) {
        if (parent[idx] < 0) return idx;
        return parent[idx] = find(parent[idx]);
    }
}
```

* `Prim`算法如下：

```java
class Solution {
    public int minCostConnectPoints(int[][] points) {
        int res = 0;
        int n = points.length;
        // 加入最小树标识
        boolean[] flag = new boolean[n];
        // 当前权重
        int[] weight = new int[n];
        int[] preNode = new int[n];
        Arrays.fill(weight, Integer.MAX_VALUE);

        int start = 0;
        weight[start] = 0;
        for (int i = 1; i < n; i++) {
            // 扩展距离
            for (int j = 0; j < n; j++) {
                int dis = Math.abs(points[start][0] - points[j][0]) + Math.abs(points[start][1] - points[j][1]);
                // 更新距离
                if (!flag[j] && weight[j] > dis) {
                    weight[j] = dis;
                    preNode[j] = start;
                }
            }
            flag[start] = true;

            // 寻找下一个start，并加入当前最短距离
            for (int j = 0; j < n; j++) {
                if (flag[j]) continue;
                if (flag[start] || weight[start] > weight[j]) start = j;
            }
            res += weight[start];
            weight[start] = Integer.MAX_VALUE;
        }
        return res;
    }
    
}
```

## 性能分析

&emsp;`Kruskal`算法时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。



&emsp;`Prim`算法时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。



## 官方解题

&emsp;