[toc]

There are a number of spherical balloons spread in two-dimensional space. For each balloon, provided input is the start and end coordinates of the horizontal diameter. Since it's horizontal, y-coordinates don't matter and hence the x-coordinates of start and end of the diameter suffice. Start is always smaller than end. There will be at most $10^4$ balloons.

An arrow can be shot up exactly vertically from different points along the x-axis. A balloon with $x_{\text{start}}$ and $x_\text{end}$ bursts by an arrow shot at x if $x_\text{start} \le x \le x_\text{end}$. There is no limit to the number of arrows that can be shot. An arrow once shot keeps travelling up infinitely. The problem is to find the minimum number of arrows that must be shot to burst all balloons.



## 题目解读

&emsp;给定水平空间气球的区间，无需关注垂直方向，求，射爆所有气球所需的最少的箭。

```java
class Solution {
    public int findMinArrowShots(int[][] points) {

    }
}
```

## 程序设计

* 问题实际就是区间交集的数目，核心思路为贪心法，可用贪心法证明，对于复杂情况`A`与`B`交集一部分，`B`与`C`交集一部分，`A`与`C`不相交，`C`与`D`交集一部分，`D`与`A`不相交；只观察两个气球，根据贪心法，需要在`A`、`B`相交区间射箭，假设最优方法不在`A`、`B`区间射，则`B`与`C`区间需要射箭，若`D`与`B`相交则总的射箭数不变，若不相交，则`D`需要额外射箭，数目超过贪心法，假设失败，贪心法是最优。

```java
class Solution {
    public int findMinArrowShots(int[][] points) {
        if (points == null || points.length == 0) return 0;

        // 排序
        Arrays.sort(points, (a, b) -> a[0] - b[0]);

        int count = 0;
        int left = points[0][0], right = points[0][1];
        for (int[] point : points) {
            // 超出交集区间，更新新区间
            if (right < point[0]) {
                count++;
                left = point[0];
                right = point[1];
            } 
            // 更新交集区间
            else {
                left = Math.max(left, point[0]);
                right = Math.min(right, point[1]);
            }
        }
		
        // 需要加上最后一个区间
        return count + 1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：23ms，在所有java提交中击败了47.09%的用户。

内存消耗：47.4MB，在所有java提交中击败了83.33%的用户。

## 官方解题

&emsp;同上，社区思路一致，实现更简洁。

```java
class Solution {
    public int findMinArrowShots(int[][] points) {
        if (points.length == 0) return 0;
        Arrays.sort(points, (a, b) -> a[1] - b[1]);
        
        int count = 1;
        int x_end = points[0][1];
        for (int[] point : points) {
            if (point[0] > x_end) {
                count++;
                x_end = point[1];
            }
        }
        
        return count;
    }
}
```