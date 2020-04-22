[toc]

Find the total area covered by two rectilinear rectangles in a 2D plane.

Each rectangle is defined by its bottom left corner and top right corner as shown in the figure.



Note:

Assume that the total area is never beyond the maximum possible value of int.



## 题目解读

&emsp;求解两个矩形面积。

```java
class Solution {
    public int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
        
    }
}
```

## 程序设计

* 画图分析数学规律，得到相交条件和坐标。

```java
class Solution {
    public int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
        int sum = compute(A, B, C, D) + compute(E, F, G, H);
        int union = 0;

        // 存在重叠
        if (A < G && C > E && H > B && F < D) {
            // 重叠面积
            int x1 = Math.max(A, E);
            int x2 = Math.min(C, G);
            int y1 = Math.max(B, F);
            int y2 = Math.min(D, H);
            union = compute(x1, y1, x2, y2);
        }

        return sum - union;
    }

    // 计算单个矩形
    private int compute(int A, int B, int C, int D) {
        return (C - A) * (D - B);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：3ms，在所有java提交中击败了99.58%的用户。

内存消耗：38.9MB，在所有java提交中击败了25.00%的用户。

## 官方解题

&emsp;暂无，密切关注。