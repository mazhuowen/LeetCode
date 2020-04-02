[toc]

Given n points on a 2D plane, find if there is such a line parallel to y-axis that reflect the given points.



**Follow up:**
Could you do better than $O(n^2)$ ?



## 题目解读

&emsp;给定点，判断是否存在一条平行于`y`轴的直线使得这些点关于该直线对称。

```java
class Solution {
    public boolean isReflected(int[][] points) {

    }
}
```

## 程序设计

* 首先遍历计算所有坐标的`x`轴坐标得到可能的对称线坐标，然后判断每个点是否关于该对称线对称。
* 题目中没有明确提及，但是输入存在重复的点，需要处理。

```java
class Solution {
    public boolean isReflected(int[][] points) {
        if (points == null || points.length <= 1) return true;

        // 记录结点
        Set<Point> record = new HashSet<>();
		// 遍历计算对称线的x轴坐标，并加入集合去重
        int baseX = 0;
        for (int[] point : points) {
            Point cur = new Point(point[0], point[1]);
            if (!record.contains(cur)) {
                baseX += point[0];
                record.add(cur);
            }
        }
        // 由于点坐标是整数，对称线x轴坐标要么是整数，要么是小数部分是0.5的小数，即其2的倍数必然是整数
        if (2 * baseX % record.size() != 0) return false;
        baseX = 2 * baseX / record.size();
        // 遍历判断关于该直线是否存在对称点
        for (Point point : record) {
            Point temp = new Point(baseX - point.x, point.y);
            // 需注意坐标点在对称直线上的点
            if (point.x != baseX && !record.contains(temp)) {
                return false;
            }
        }

        return true;
    }
}

// 结点类，重写hash函数
class Point {
    int x;
    int y;

    Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Point point = (Point) o;
        return x == point.x && y == point.y;
    }

    @Override
    public int hashCode() {
        return Objects.hash(x, y);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：7ms，在所有java提交中击败了85.29%的用户。

内存消耗：41.7MB，在所有java提交中击败了40.00%的用户。

## 官方解题

&emsp;暂无，密切关注。