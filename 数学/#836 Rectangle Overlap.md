[toc]

A rectangle is represented as a list `[x1, y1, x2, y2]`, where `(x1, y1)` are the coordinates of its bottom-left corner, and `(x2, y2)` are the coordinates of its top-right corner.

Two rectangles overlap if the area of their intersection is positive.  To be clear, two rectangles that only touch at the corner or edges do not overlap.

Given two (axis-aligned) rectangles, return whether they overlap.



Notes:

* Both rectangles `rec1` and `rec2` are lists of 4 integers.
* All coordinates in rectangles will be between $-10^9$ and $10^9$.



## 题目解读

&emsp;判断矩形是否重叠。

```java
class Solution {
    public boolean isRectangleOverlap(int[] rec1, int[] rec2) {

    }
}
```

## 程序设计

* [#223 Rectangle Area](./#223 Rectangle Area.md)的简化问题。

```java
class Solution {
    public boolean isRectangleOverlap(int[] rec1, int[] rec2) {
        return rec1[0] < rec2[2] && rec2[0] < rec1[2] && rec1[1] < rec2[3] && rec2[1] < rec1[3];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;同上。