[toc]

There is a brick wall in front of you. The wall is rectangular and has several rows of bricks. The bricks have the same height but different width. You want to draw a vertical line from the **top** to the **bottom** and cross the **least** bricks.

The brick wall is represented by a list of rows. Each row is a list of integers representing the width of each brick in this row from left to right.

If your line go through the edge of a brick, then the brick is not considered as crossed. You need to find out how to draw the line to cross the least bricks and return the number of crossed bricks.

**You cannot draw a line just along one of the two vertical edges of the wall, in which case the line will obviously cross no bricks**.



**Note**:

* The width sum of bricks in different rows are the same and won't exceed INT_MAX.
* The number of bricks in each row is in range $[1,10000]$. The height of wall is in range $[1,10000]$. Total number of bricks of the wall won't exceed $20000$.



## 题目解读

&emsp;有一堵砖墙，每行砖的宽度和数目不等，现在需要画一条竖线使得穿过的砖最少，求该数值。

```java
class Solution {
    public int leastBricks(List<List<Integer>> wall) {

    }
}
```

## 程序设计

* 计数砖缝数目，选择最大的砖缝划线，需注意避开最后一块转，因为不能在两侧边缘划线。

```java
class Solution {
    public int leastBricks(List<List<Integer>> wall) {
        if (wall == null || wall.size() == 0) return 0;
        int n = wall.size(), max = 0;
        // 记录砖缝数目
        Map<Integer, Integer> counter = new HashMap<>();
        for (List<Integer> brick : wall) {
            // 初始化砖缝
            int pre = 0;
            // 丢弃最后一块砖，因为最后砖缝是边缘，不符合题目要求
            for (int i = 0; i < brick.size() - 1; i++) {
                pre += brick.get(i);
                // 计数
                counter.put(pre, counter.getOrDefault(pre, 0) + 1);
                max = Math.max(max, counter.get(pre));
            }
        }
        return n - max;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：17ms，在所有java提交中击败了45.47%的用户。

内存消耗：42.3MB，在所有java提交中击败了16.27%的用户。

## 官方解题

&emsp;官方思路类似。