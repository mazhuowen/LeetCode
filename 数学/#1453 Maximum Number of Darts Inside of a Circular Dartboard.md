[toc]

You have a very large square wall and a circular dartboard placed on the wall. You have been challenged to throw darts into the board blindfolded. Darts thrown at the wall are represented as an array of `points` on a 2D plane. 

Return the maximum number of points that are within or lie on **any** circular dartboard of radius `r`.



**Constraints**:

* $1 \le \text{points.length} \le 100$
* $\text{points[i].length} == 2$
* $-10^4 \le \text{points[i][0], points[i][1]} \le 10^4$
* $1 \le r \le 5000$



## 题目解读

&emsp;求落在指定半径靶子上的最多飞镖数。

```java
class Solution {
    public int numPoints(int[][] points, int r) {

    }
}
```

## 程序设计

* 由于问题规模较少，可以两两坐标确定两个圆心，然后暴力遍历判断点在园内的数目，取最大值即可。
* 在圆内的判断较为简单，问题主要在于圆心的计算；如果使用代数法计算，会使得程序难以实现和阅读，可采用几何法来计算；每两个点对应两个圆心，以圆心`A`横坐标为例，$x_A = x_{mid} - \vert\text{CE}\vert = x_mid - \vert\text{AC}\vert\cos{\alpha}$，而$\cos{\alpha} = \cos{\beta} = \frac{\vert\text{CD}\vert}{\vert\text{CG}\vert}$，纵坐标同理，$y_A = y_{mid} + \vert\text{AE}\vert$。

<img src="..\images\#1453.png" style="zoom:80%;" />

```java
class Solution {
    public int numPoints(int[][] points, int r) {
        if (points == null || points.length == 0) return 0;
        int res = 0;
        for (int i = 0; i < points.length; i++) {
            for (int j = i + 1; j < points.length; j++) {
                // 无法构成圆
                if (getSquareDistance(points[i][0], points[i][1], points[j][0], points[j][1]) - 4 * r * r > 0.0001D) continue;

                double[][] centers = getCycleCenter(points[i][0], points[i][1], points[j][0], points[j][1], r);
                for (double[] center : centers) {
                    // 统计
                    int count = 0;
                    for (int[] point : points) {
                        if (isInCycle(center, r, point[0], point[1])) count++;
                    }
                    res = Math.max(res, count);
                }
            }
        }
        // 0表示任意两个点无法构成指定半径的圆，只能包含一个点
        return res == 0 ? 1 : res;
    }

    // 根据两个点计算圆心（有两个）
    private double[][] getCycleCenter(int a, int b, int c, int d, int r) {
        double[][] res = new double[2][2];
        double midX = (a + c) / 2.0, midY = (b + d) / 2.0;
        double gc2 = (a - midX) * (a - midX) + (b - midY) * (b - midY);
        double ac2 = r * r - gc2;
        // 根据几何学计算圆心坐标
        double centerX = midX - Math.sqrt(ac2) * Math.abs(midY - b) / Math.sqrt(gc2);
        double centerY = midY + Math.sqrt(ac2) * Math.abs(midX - a) / Math.sqrt(gc2);
        res[0] = new double[]{centerX, centerY};
        // 圆心2与圆心1对称
        res[1] = new double[]{a + c - centerX, b + d - centerY};
        return res;
    }

    // 返回距离平方
    private double getSquareDistance(double a, double b, double c, double d) {
        return (a - c) * (a - c) + (b - d) * (b - d);
    }

    private boolean isInCycle(double[] center, int r, int a, int b) {
        double dis = getSquareDistance(a, b, center[0], center[1]);
        return dis - r * r <= 0.00001D;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^3)$，空间复杂度为$O(1)$。

执行用时：35ms，在所有java提交中击败了49.56%的用户。

内存消耗：39MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区总体思路类似，计算圆心方法有差异，有采用向量计算的方式。