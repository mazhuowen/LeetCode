[toc]

Given a circle represented as (`radius`, `x_center`, `y_center`) and an axis-aligned rectangle represented as (`x1`, `y1`, `x2`, `y2`), where (`x1`, `y1`) are the coordinates of the bottom-left corner, and (`x2`, `y2`) are the coordinates of the top-right corner of the rectangle.

Return True if the circle and rectangle are overlapped otherwise return False.

In other words, check if there are **any** point (xi, yi) such that belongs to the circle and the rectangle at the same time.

 

**Example 1**:

<img src="..\images\#1401_exp1.png"  />

```
Input: radius = 1, x_center = 0, y_center = 0, x1 = 1, y1 = -1, x2 = 3, y2 = 1
Output: true
Explanation: Circle and rectangle share the point (1,0) 
```

**Example 2**:

<img src="..\images\#1401_exp2.png"  />

```
Input: radius = 1, x_center = 0, y_center = 0, x1 = -1, y1 = 0, x2 = 0, y2 = 1
Output: true
```

**Example 3**:

<img src="..\images\#1401_exp3.png"  />

```
Input: radius = 1, x_center = 1, y_center = 1, x1 = -3, y1 = -3, x2 = 3, y2 = 3
Output: true
```

**Example 4**:

```
Input: radius = 1, x_center = 1, y_center = 1, x1 = 1, y1 = -3, x2 = 2, y2 = -1
Output: false
```



**Constraints**:

* $1 \le \text{radius} \le 2000$
* $-10^4 \le \text{x_center, y_center, x1, y1, x2, y2} \le 10^4$
* $x_1 < x_2$
* $y_1 < y_2$



## 题目解读

&emsp;判断圆与矩形是否相交。

```java
class Solution {
    public boolean checkOverlap(int radius, int x_center, int y_center, int x1, int y1, int x2, int y2) {
        
    }
}
```

## 程序设计

* 最基本的思路是寻找矩形中离圆最近的点，然后计算距离圆心的距离。

```java
class Solution {
    public boolean checkOverlap(int radius, int x_center, int y_center, int x1, int y1, int x2, int y2) {
        // 在矩形上寻找距离圆最近的点
        int x, y;
        if (x_center <= x1) x = x1;
        else if (x_center >= x2) x = x2;
        else x = x_center;
        if (y_center <= y1) y = y1;
        else if (y_center >= y2) y = y2;
        else y = y_center; 

        return (y - y_center) * (y - y_center) + (x - x_center) * (x - x_center) <= radius * radius;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：35.4 MB, 在所有 Java 提交中击败了53.33%的用户。

## 官方解题

&emsp;暂无，密切关注。