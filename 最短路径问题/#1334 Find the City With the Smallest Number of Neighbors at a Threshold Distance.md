[toc]

There are $n$ cities numbered from $0$ to $n-1$. Given the array `edges` where `edges[i] = [fromi, toi, weighti]` represents a bidirectional and weighted edge between cities `fromi` and `toi`, and given the integer `distanceThreshold`.

Return the city with the smallest number of cities that are reachable through some path and whose distance is **at most** `distanceThreshold`, If there are multiple such cities, return the city with the greatest number.

Notice that the distance of a path connecting cities i and j is equal to the sum of the edges' weights along that path.



**Constraints**:

* $2 \le n \le 100$
* $1 \le \text{edges.length} \le n * (n - 1) / 2$
* $\text{edges[i].length} == 3$
* $0 \le fromi < toi < n$
* $1 \le \text{weighti, distanceThreshold} \le 10^4$
* All pairs `(fromi, toi)` are distinct.



## 题目解读

&emsp;计算$n$个城镇之间的距离，统计每个城镇到其它城镇的距离小于给定阈值的个数，返回最小的个数的城镇，如果相同则返回编号大的城镇。

```java
class Solution {
    public int findTheCity(int n, int[][] edges, int distanceThreshold) {

    }
}
```

## 程序设计

* 顶点对最短路径的典型应用，计算所有城镇之间的距离，然后统计即可。

```java
class Solution {
    public int findTheCity(int n, int[][] edges, int distanceThreshold) {
        // 距离矩阵
        int[][] distance = new int[n][n];
        // 前驱矩阵
        int[][] prev = new int[n][n];
        // 从边集初始化（由于是无向图，题目给定的是上三角，构图时补充下三角）
        for (int[] edge : edges) {
            distance[edge[0]][edge[1]] = edge[2];
            distance[edge[1]][edge[0]] = edge[2];
            prev[edge[0]][edge[1]] = edge[0];
            prev[edge[1]][edge[0]] = edge[1];
        }
        // 将不存在边的位置置为无限大（题目中不超过10^7），前驱置为-1
        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                if (i != j && distance[i][j] == 0) {
                    distance[i][j] = 1_000_000_000;
                    distance[j][i] = 1_000_000_000;
                    prev[i][j] = -1;
                    prev[j][i] = -1;
                }
            }
        }

        // Floyd算法（由于是无向图，只计算上三角，然后同步到下三角）
        for (int k = 0; k < n; k++) {
            for (int i = 0; i < n; i++) {
                for (int j = i; j < n; j++) {
                    // 更新，同时更新前驱
                    if (distance[i][j] > distance[i][k] + distance[k][j]) {
                        distance[i][j] = distance[i][k] + distance[k][j];
                        distance[j][i] = distance[i][k] + distance[k][j];
                        prev[i][j] = prev[k][j];
                        prev[j][i] = prev[k][j];
                    }
                }
            }
        }
        // 统计小于阈值的最小城市
        int minCity = -1, minCount = Integer.MAX_VALUE;
        for (int i = 0; i < n; i++) {
            // 计数
            int count = 0;
            for (int j = 0; j < n; j++) {
                if (distance[i][j] <= distanceThreshold) count++;
            }
			// 计数小于等于阈值，更新
            if (count <= minCount) {
                minCity = i;
                minCount = count;
            }
        }
        return minCity;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(V)$，空间复杂度为$O(V^2)$。

执行用时：8ms，在所有java提交中击败了98.92%的用户。

内存消耗：41.1MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。