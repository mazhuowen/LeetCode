[toc]

Given the radius and x-y positions of the center of a circle, write a function `randPoint` which generates a uniform random point in the circle.



**Note**:

* input and output values are in `floating-point`.
* radius and x-y position of the center of the circle is passed into the class constructor.
* a point on the circumference of the circle is considered to be in the circle.
* `randPoint` returns a size 2 array containing x-position and y-position of the random point, in that order.



## 题目解读

&emsp;给定圆心和半径，均匀采样圆中的点。

```java
class Solution {

    public Solution(double radius, double x_center, double y_center) {

    }
    
    public double[] randPoint() {

    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(radius, x_center, y_center);
 * double[] param_1 = obj.randPoint();
 */
```

## 程序设计

* 最基本的思路是参考蒙特卡洛采样估计$\pi$值，即在一个正方形内采样坐标点，如果在圆外则继续采样。

```java
class Solution {
    double xc, yc, r;

    public Solution(double radius, double x_center, double y_center) {
        this.r = radius;
        this.xc = x_center;
        this.yc = y_center;
    }

    public double[] randPoint() {
        double x0 = xc - r, y0 = yc - r;
        while (true) {
            // 采样正方形内的点
            double newX = x0 + 2 * r * Math.random();
            double newY = y0 + 2 * r * Math.random();
            // 在圆内，返回
            if (Math.pow(newX - xc, 2) + Math.pow(newY - yc, 2) <= Math.pow(r, 2))
                return new double[]{newX, newY};
        }
    }
}
```

## 性能分析

&emsp;最坏会一直循环下去，由于圆与正方形面积占比为$\frac{\pi r^2}{4r^2}$，期望为$\frac{4}{\pi}$次；空间复杂度为$O(1)$。

执行用时：148ms，在所有java提交中击败了96.72%的用户。

内存消耗：48.8MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;除了上述思路，官方还提供了极坐标的思路。由于圆内点可表示为$(x\cos\theta)^2 + (x\sin\theta)^2 \le r^2$，即$(x\cos\theta)^2 + (x\sin\theta)^2 = r^2 \times\text{random} \implies x = \sqrt{\text{random}}r,\ \theta = 2\pi \times \text{random}$。

```java
class Solution {
    double xc, yc, r;

    public Solution(double radius, double x_center, double y_center) {
        this.r = radius;
        this.xc = x_center;
        this.yc = y_center;
    }

    public double[] randPoint() {
        double len = r * Math.sqrt(Math.random());
        double theta = 2 * Math.PI * Math.random();
        return new double[]{len * Math.cos(theta) + xc, len * Math.sin(theta) + yc};
    }
}
```

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：151ms，在所有java提交中击败了92.35%的用户。

内存消耗：48.6MB，在所有java提交中击败了100.00%的用户。