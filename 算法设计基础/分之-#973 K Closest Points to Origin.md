[toc]

We have a list of `points` on the plane.  Find the `K` closest points to the origin $(0, 0)$.

(Here, the distance between two points on a plane is the Euclidean distance.)

You may return the answer in any order.  The answer is guaranteed to be unique (except for the order that it is in.)

 Note:

* $1 \le K <= \text{points.length} \le 10000$
* $-10000 < \text{points[i][0]} < 10000$
* $-10000 < \text{points[i][1]} < 10000$



## 题目解读

&emsp;给定二维平面点左边，得到离原点最近的$K$个点。

```java
class Solution {
    public int[][] kClosest(int[][] points, int K) {

    }
}
```

## 程序设计

* 最简单的思路就是计算所有的坐标然后排序。


```java
class Solution {
    public int[][] kClosest(int[][] points, int K) {
        if(points == null || points.length == 0 || K == 0) {
            return new int[][]{};
        }
        if(K >= points.length) {
            return points;
        }
        // 计算，数组保存距离和索引信息
        Integer[][] distance = new Integer[points.length][2];
        for(int i = 0; i < points.length; i++) {
            distance[i] = new Integer[]{square(points[i][0]) + square(points[i][1]), i};
        }
        // 排序返回前K个
        Arrays.sort(distance, (a, b) -> a[0] - b[0]);
        int[][] res = new int[K][2];
        for(int i = 0; i < K; i++) {
            res[i] = points[distance[i][1]];
        }
        return res;
    }

    private int square(int val) {
        return val * val;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：51ms，在所有java提交中击败了26.93%的用户。

内存消耗：49.3MB，在所有java提交中击败了100.00%的用户。

## 官方解题

