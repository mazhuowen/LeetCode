[toc]

In a row of `seats`, `1` represents a person sitting in that seat, and `0` represents that the seat is empty. 

There is at least one empty seat, and at least one person sitting.

Alex wants to sit in the seat such that the distance between him and the closest person to him is maximized. 

Return that maximum distance to closest person.



**Constraints**:

* $2 \le \text{seats.length} \le 20000$
* `seats` contains only 0s or 1s, at least one `0`, and at least one `1`.



## 题目解读

&emsp;求新来的人坐到离两边人最远的位置时，离最近的人的距离。

```java
class Solution {
    public int maxDistToClosest(int[] seats) {

    }
}
```

## 程序设计

* 记录左边的人和右边的人，最大区间的最小距离距离为左右区间的一半；需要注意左边为空位及右边为空位的特殊情况。

```java
class Solution {
    public int maxDistToClosest(int[] seats) {
        if (seats == null || seats.length < 2) throw new IllegalArgumentException("invalid param");

        int left = -1, maxLen = 0;
        for (int i = 0; i < seats.length - 1; i++) {
            if (seats[i] == 1 && seats[i + 1] == 0) left = i;
            else if (seats[i] == 0 && seats[i + 1] == 1) {
                // 左边为空位
                if (left == -1) maxLen = Math.max(maxLen, i + 1);
                else maxLen = Math.max(maxLen, (i + 1 - left) / 2);
            }
        }

        // 右边为空位
        if (seats[seats.length - 1] == 0) maxLen = Math.max(maxLen, seats.length - 1 - left);

        return maxLen;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：3ms，在所有java提交中击败了54.09%的用户。

内存消耗：42MB，在所有java提交中击败了11.11%的用户。

## 官方解题

&emsp;同上。