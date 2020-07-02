[toc]

You are given an array `x` of $n$ positive numbers. You start at point `(0,0)` and moves `x[0]` metres to the north, then `x[1]` metres to the west, `x[2]` metres to the south, `x[3]` metres to the east and so on. In other words, after each move your direction changes counter-clockwise.

Write a one-pass algorithm with O`(1)` extra space to determine, if your path crosses itself, or not.



## 题目解读

&emsp;判断是否路径交叉。

```java
class Solution {
    public boolean isSelfCrossing(int[] x) {

    }
}
```

## 程序设计

* 参考社区思路，螺旋有三种情况，及向外螺旋，向内螺旋，向外螺旋转变为向内螺旋，则模拟判断是否符合螺旋条件，不符合说明相交。
* 向内、向外螺旋判断较为简单，外转内分如下情况：
  * `idx`对应的高度小于等于`idx - 2`但高于`idx-2`到`idx - 4`之间的间隙，则只能在`idx - 1`减去`idx - 3`的区域螺旋，需要更新长度。
  * `idx`对应的高度低于间隙，则在`idx-1`的区域内螺旋，无需改动。

<img src="..\images\#335.png" style="zoom:150%;" />

```java
class Solution {
    public boolean isSelfCrossing(int[] x) {
        if (x == null || x.length < 4) return false;

        // 起始是否向外旋转
        boolean outRolling = x[0] < x[2];
        int idx = 3;
        while (outRolling && idx < x.length) {
            // 符合外旋转的条件，继续遍历
            if (x[idx] > x[idx - 2]) idx++;
            else {
                // 转变为内循环退出
                int gap = idx >= 4 ? x[idx - 2] - x[idx - 4] : x[idx - 2];
                // 两种内循环，其中一种需要更新距离
                if (x[idx] >= gap)  x[idx - 1] = x[idx - 1] - x[idx - 3];
                idx++;
                break;
            }
        }

        // 内循环
        while (idx < x.length) {
            if (x[idx] < x[idx - 2]) idx++;
            // 相交
            else return true;
        }
        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.2MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区还有采用边界条件判断的思路，但不如模拟直接。