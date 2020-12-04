[toc]

Given a set of points in the xy-plane, determine the minimum area of **any** rectangle formed from these points, with sides **not necessarily parallel** to the x and y axes.

If there isn't any rectangle, return $0$.

 

**Example 1**:

<img src="..\images\#963_exp1.png" style="zoom: 50%;" />

```
Input: [[1,2],[2,1],[1,0],[0,1]]
Output: 2.00000
Explanation: The minimum area rectangle occurs at [1,2],[2,1],[1,0],[0,1], with an area of 2.
```


**Example 2**:

<img src="..\images\#963_exp2.png" style="zoom: 70%;" />

```
Input: [[0,1],[2,1],[1,1],[1,0],[2,0]]
Output: 1.00000
Explanation: The minimum area rectangle occurs at [1,0],[1,1],[2,1],[2,0], with an area of 1.
```

**Example 3**:

<img src="..\images\#963_exp3.png" style="zoom: 35%;" />

```
Input: [[0,3],[1,2],[3,1],[1,3],[2,1]]
Output: 0
Explanation: There is no possible rectangle to form from these points.
```

**Example 4**:

<img src="..\images\#963_exp4.png" style="zoom: 80%;" />

```
Input: [[3,1],[1,1],[0,1],[2,1],[3,3],[3,2],[0,2],[2,3]]
Output: 2.00000
Explanation: The minimum area rectangle occurs at [2,1],[2,3],[3,3],[3,1], with an area of 2.
```



**Note**:

* $1 \le \text{points.length} \le 50$
* $0 \le \text{points[i][0]} \le 40000$
* $0 \le \text{points[i][1]} \le 40000$
* All points are distinct.
* Answers within $10^{-5}$ of the actual value will be accepted as correct.



## 题目解读

&emsp;给定点，寻找可以构成的最小矩形面积。

```java
class Solution {
    public double minAreaFreeRect(int[][] points) {

    }
}
```

## 程序设计

* 最基本的思路是遍历尝试三个顶点，判断第四个顶点是否存在，边是否垂直，然后计算比较，时间复杂度为$O(N^3)$；
* 如果提前计算边的斜率，需要在斜率及垂直斜率中遍历匹配，任然繁琐；转换思路。由于矩形对角线的交点到各顶点的距离是相等的，如果两对对角线长度相等且中心点为同一个，则必然可以组成一个矩形；
* 根据上述分析，记录相等对角线长度且中心点相等的对角线列表，对于同一矩形，只需在同一列表中对角线两两匹配；对角线的保存采用中心节点、端点的字典对。

```java
class Solution {
    public double minAreaFreeRect(int[][] points) {
        int n = points.length;
        Point[] arr = new Point[n];
        for (int i = 0; i < n; i++) {
            arr[i] = new Point(points[i][0], points[i][1]);
        }

        // 保存直径长度对应的线段中点和端点
        Map<Integer, Map<Point, List<Point>>> record = new HashMap<>();
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                // 为保存精度，不除以2
                Point center = new Point(arr[i].x + arr[j].x, arr[i].y + arr[j].y);
                // 直径平方
                int r2 = (arr[i].x - arr[j].x) * (arr[i].x - arr[j].x) + (arr[i].y - arr[j].y) * (arr[i].y - arr[j].y);

                if (!record.containsKey(r2)) record.put(r2, new HashMap<>());
                if (!record.get(r2).containsKey(center)) record.get(r2).put(center, new LinkedList<>());

                // 加入第一个端点（因为直到中心可以唯一确定第二个端点，故不加入第二个端点）
                record.get(r2).get(center).add(arr[i]);
            }
        }

        double res = Double.MAX_VALUE;
        // 遍历每个直径的中心点
        for (Map<Point, List<Point>> centerMap : record.values()) {
            // 遍历每个中心
            for (Map.Entry<Point, List<Point>> entry : centerMap.entrySet()) {
                // 对角线的数目
                int len = entry.getValue().size();
                // 由于中心点相同，将对角线两两组合为矩形
                for (int i = 0; i < len; i++) {
                    // 获取对角线两个端点
                    Point P = entry.getValue().get(i);
                    Point T = new Point(entry.getKey().x - P.x, entry.getKey().y - P.y);
                    for (int j = i + 1; j < len; j++) {
                        Point Q = entry.getValue().get(j);
                        res = Math.min(res, P.distance(Q) * T.distance(Q));
                    }
                }
            }
        }
        return res == Double.MAX_VALUE ? 0 : res;
    }
}

class Point {
    int x, y;

    Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public double distance(Point that) {
        return Math.sqrt((this.x - that.x) * (this.x - that.x) + (this.y - that.y) * (this.y - that.y));
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (!(obj instanceof Point)) return false;
        Point that = (Point)obj;
        return this.x == that.x && this.y == that.y;
    }

    @Override
    public int hashCode() {
        return Objects.hash(x, y);
    }
}
```

## 性能分析

&emsp;被分在同一中心点同一长度的列表不超过$\log_2N$，故时间复杂度为$O(N^2\log_2N)$，空间复杂度为$O(N^2)$。

执行用时：32 ms, 在所有 Java 提交中击败了68.13%的用户

内存消耗：39.3 MB, 在所有 Java 提交中击败了34.09%的用户。

## 官方解题

&emsp;上述思路参考官方解题。