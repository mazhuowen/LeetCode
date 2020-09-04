[toc]

Table `point_2d` holds the coordinates `(x,y)` of some unique points (more than two) in a plane.


Write a query to find the shortest distance between these points rounded to 2 decimals.

```
| x    | y    |
| ---- | ---- |
| -1   | -1   |
| 0    | 0    |
| -1   | -2   |
```

The shortest distance is 1.00 from point `(-1,-1)` to `(-1,2)`. So the output should be:

```
| shortest |
| -------- |
| 1.00     |
```



**Note**: The longest distance among all the points are less than 10000.



## 题目解读

&emsp;求解最短距离。

## 程序设计

* 直接级联计算查询。

```mysql
SELECT min(round(sqrt(pow(p1.x - p2.x, 2) + pow(p1.y - p2.y, 2)), 2)) AS shortest FROM point_2d p1, point_2d p2 where p1.x != p2.x OR p1.y != p2.y
```

## 性能分析

执行用时：170ms，在所有MySQL提交中击败了89.98%的用户。

内存消耗：0B，在所有MySQL提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。