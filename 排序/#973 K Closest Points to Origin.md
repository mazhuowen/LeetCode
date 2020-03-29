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

* 事实上可以采用分治的思想，结合快速排序，边计算边排序，不同与原生快速排序，本题只关注前$K$个，这意味着我们在划分区间的时候不需要遍历所有数组元素，根据划分点判断是否超过$K$个，然后缩小换分范围，这使得时间性能大大提高。

```java
class Solution {
    public int[][] kClosest(int[][] points, int K) {
        quickSort(points, K, 0, points.length - 1);
        return Arrays.copyOf(points, K);
    }

    private void quickSort(int[][] point, int K, int left, int right) {
        int start = left, end = right;
        // 递归终止条件
        if (start >= end) return;
        // 基准切分点
        int[] basePoint = point[start];
        int dis = distance(basePoint);
        while (start < end) {
            while (start < end && distance(point[end]) >= dis)
                end--;

            if (start < end) point[start++] = point[end];
            while (start < end && distance(point[start]) < dis)
                start++;
            
            if (start < end) point[end--] = point[start];
        }
        point[start] = basePoint;

        // 正好划分了K个，不必继续排序
        if (start == K - 1) return;
        // 左半部分超过K个，只排序左半部分
        if (start >= K) {
            quickSort(point, K, left, start - 1);
        } 
        // 左半部分少于k个，左半部分已满足要求，切分右半部分
        else {
            quickSort(point, K, start + 1, right);
        }
    }

    private int distance(int[] point) {
        return point[0] * point[0] + point[1] * point[1];
    }
}
```

## 性能分析

&emsp;枚举法时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：51ms，在所有java提交中击败了26.93%的用户。

内存消耗：49.3MB，在所有java提交中击败了100.00%的用户。

&emsp;快速排序时间复杂度为$O(N\log_2N)$，在本题中由于侧重于划分而非排序，每次并不会遍历所有数组元素，只会遍历一半，平均情况下$T(N) = N + T(\frac{N}{2})$，可得$T(N) = N + \frac{N}{2} + \frac{N}{4} + \dots + 1 = N * (\frac{1 - \frac{1}{2N}}{1 - \frac{1}{2}}) = 2 * N - 1$，即时间复杂度为$O(N)$；空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：3ms，在所有java提交中击败了100.00%的用户。

内存消耗：44.1MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。