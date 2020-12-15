[toc]

You are given an array `coordinates, coordinates[i] = [x, y]`, where `[x, y]` represents the coordinate of a point. Check if these points make a straight line in the XY plane.

 

**Example 1**:

<img src="..\images\#1232_exp1.jpg" style="zoom:80%;" />

```
Input: coordinates = [[1,2],[2,3],[3,4],[4,5],[5,6],[6,7]]
Output: true
```

**Example 2**:

<img src="..\images\#1232_exp2.jpg" style="zoom:80%;" />

```
Input: coordinates = [[1,1],[2,2],[3,4],[4,5],[5,6],[7,7]]
Output: false
```



**Constraints**:

* $2 \le \text{coordinates.length} \le 1000$
* $\text{coordinates[i].length} == 2$
* $-10^4 \le \text{coordinates[i][0], coordinates[i][1]} \le 10^4$
* `coordinates` contains no duplicate point.



## 题目解读

&emsp;判断给定点是否在一条直线上。

```java
class Solution {
    public boolean checkStraightLine(int[][] coordinates) {

    }
}
```

## 程序设计

* 根据直线的参数表达式来判断后续点是否在前两个点组成的直线上。

```java
class Solution {
    public boolean checkStraightLine(int[][] coordinates) {
        if (coordinates == null || coordinates.length < 2) throw new IllegalArgumentException("invalid param");
        // x = x_0 + t * x_delta
        int x_0 = coordinates[0][0], y_0 = coordinates[0][1], x_delta = coordinates[1][0] - x_0, y_delta = coordinates[1][1] - y_0;
        for (int i = 2; i < coordinates.length; i++) {
            if (!check(coordinates[i][0], coordinates[i][1], x_0, y_0, x_delta, y_delta)) return false;
        }
        return true;
    }

    private boolean check(int x, int y, int x_0, int y_0, int x_delta, int y_delta) {
        return (x - x_0) * y_delta == (y - y_0) * x_delta;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：37.7 MB, 在所有 Java 提交中击败了97.87%的用户。

## 官方解题

&emsp;暂无，密切关注。社区采用斜率来判断，通过将出发转化为乘法，避免分母为$0$的特殊情况。
