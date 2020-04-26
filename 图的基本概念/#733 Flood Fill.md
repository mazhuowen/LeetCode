[toc]

An `image` is represented by a 2-D array of integers, each integer representing the pixel value of the image (from 0 to 65535).

Given a coordinate `(sr, sc)` representing the starting pixel (row and column) of the flood fill, and a pixel value `newColor`, "flood fill" the image.

To perform a "flood fill", consider the starting pixel, plus any pixels connected 4-directionally to the starting pixel of the same color as the starting pixel, plus any pixels connected 4-directionally to those pixels (also with the same color as the starting pixel), and so on. Replace the color of all of the aforementioned pixels with the newColor.

At the end, return the modified image.



Note:

* The length of image and `image[0]` will be in the range `[1, 50]`.
* The given starting pixel will satisfy `0 <= sr < image.length` and `0 <= sc < image[0].length`.
* The value of each color in `image[i][j]` and `newColor` will be an integer in `[0, 65535]`.



## 题目解读

&emsp;渲染图片，从指定位置渲染连续相同像素值的像素为新的值。

```java
class Solution {
    public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {

    }
}
```

## 程序设计

* 从指定点深度优先搜索。

```java
class Solution {
    int[] delta = new int[]{1, 0, -1, 0, 1};

    public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
        if (image[sr][sc] == newColor) return image;
        floodFill(image, sr, sc, image[sr][sc], newColor);
        return image;
    }

    private void floodFill(int[][] image, int sr, int sc, int oldColor, int newColor) {
        // 不符合条件
        if (sr < 0 || sr >= image.length || sc < 0 || sc >= image[0].length
                || image[sr][sc] != oldColor) return;

        // 染色
        image[sr][sc] = newColor;
        for (int i = 0; i < 4; i++) {
            floodFill(image, sr + delta[i], sc + delta[i + 1], oldColor, newColor);
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：1ms，在所有java提交中击败了94.98%的用户。

内存消耗：40.7MB，在所有java提交中击败了60.00%的用户。

## 官方解题

&emsp;同上。