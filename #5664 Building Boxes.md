[toc]

You have a cubic storeroom where the width, length, and height of the room are all equal to n units. You are asked to place n boxes in this room where each box is a cube of unit side length. There are however some rules to placing the boxes:

You can place the boxes anywhere on the floor.
If box x is placed on top of the box y, then each side of the four vertical sides of the box y must either be adjacent to another box or to a wall.
Given an integer n, return the minimum possible number of boxes touching the floor.

 

Example 1:



Input: n = 3
Output: 3
Explanation: The figure above is for the placement of the three boxes.
These boxes are placed in the corner of the room, where the corner is on the left side.
Example 2:



Input: n = 4
Output: 3
Explanation: The figure above is for the placement of the four boxes.
These boxes are placed in the corner of the room, where the corner is on the left side.
Example 3:



Input: n = 10
Output: 6
Explanation: The figure above is for the placement of the ten boxes.
These boxes are placed in the corner of the room, where the corner is on the back side.


Constraints:

1 <= n <= 109



## 题目解读

&emsp;

```java
class Solution {
    public int minimumBoxes(int n) {

    }
}
```

## 程序设计

* 

```java
class Solution {
    public int minimumBoxes(int n) {
        // 求饱和叠放的最高层数
        int level = 1, total = 0;
        while (total + num(level) <= n) {
            total += num(level++);
        }

        // 在饱和叠放的基础上继续叠放
        int bottom = num(--level), inc = 1;
        while (total < n) {
            // 底面积增加
            bottom++;
            // 当前增加的最多叠加数
            total += inc++;
        }
        return bottom;
    }

    // 求每一层的数目
    private int num(int level) {
        return level * (level + 1) / 2;
    }
}
```

* 

```java
class Solution {
    public int minimumBoxes(int n) {
        int level = level(n), total = (int)total(level), bottom = (int)inc(level);

        int left = 0, right = (int)Math.sqrt(2L * (n - total)) + 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (total + inc(mid) >= n) right = mid;
            else left = mid + 1;
        }
        return bottom + left;
    }

    private long total(long level) {
        return level * (level + 1) * (level + 2) / 6;
    }

    // 底增加的方块导致最终增加的总块数
    private long inc(int incBottom) {
        return incBottom * (incBottom + 1) / 2;
    }

    private int level(int n) {
        int left = 1, right = (int)Math.pow(6L * n, 1 / 3.0) + 1;
        while (left < right) {
            int mid = left + (right - left) / 2 + 1;
            if (total(mid) <= n) left = mid;
            else right = mid - 1;
        }
        return left;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O()$，空间复杂度为$O()$。



## 官方解题

&emsp;

```java

```

&emsp;时间复杂度为$O()$，空间复杂度为$O()$。
