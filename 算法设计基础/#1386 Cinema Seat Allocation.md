[toc]

<img src="..\images\#1386.png" style="zoom: 50%;" />

A cinema has $n$ rows of seats, numbered from $1$ to $n$ and there are ten seats in each row, labelled from $1$ to $10$ as shown in the figure above.

Given the array `reservedSeats` containing the numbers of seats already reserved, for example, `reservedSeats[i] = [3,8]` means the seat located in row $3$ and labelled with $8$ is already reserved.

Return the maximum number of four-person groups you can assign on the cinema seats. A four-person group occupies four adjacent seats **in one single row**. Seats across an aisle (such as `[3,3]` and `[3,4]`) are not considered to be adjacent, but there is an exceptional case on which an aisle split a four-person group, in that case, the aisle split a four-person group in the middle, which means to have two people on each side.



**Constraints**:

* $1 \le n \le 10^9$
* $1 \le \text{reservedSeats.length} \le \min(10*n, 10^4)$
* $\text{reservedSeats[i].length} == 2$
* $1 \le \text{reservedSeats[i][0]} \le n$
* $1 \le \text{reservedSeats[i][1]} \le 10$
* All `reservedSeats[i]` are distinct.



## 题目解读

&emsp;电影院有$n$排座椅，每排有十个座椅；给定已占有的座位，求最多可连续坐四人的座位数目。

```java
class Solution {
    public int maxNumberOfFamilies(int n, int[][] reservedSeats) {
        
    }
}
```

## 程序设计

* 由于每一排只有十个座位，只有十个状态，可用二进制表示座位状态。

```java
class Solution {
    // 座位状态
    private static final int LEFT = 0b1000011111;
    private static final int MIDDLE = 0b1110000111;
    private static final int RIGHT = 0b1111100001;

    public int maxNumberOfFamilies(int n, int[][] reservedSeats) {
        int max = 2 * n;
        if (reservedSeats == null || reservedSeats.length == 0) return max;

        // 记录行的占座情况
        Map<Integer, Integer> stats = new HashMap<>(); 
        for (int[] record : reservedSeats) {
            int row = record[0], s = 1 << (record[1] - 1);
            stats.put(row, stats.getOrDefault(row, 0) | s);
        }

        // 减去占用的行
        int res = max - 2 * stats.size();
        for (int stat : stats.values()) {
            // 左端和右端都可坐四人
            if ((stat | LEFT) == LEFT && (stat | RIGHT) == RIGHT) res += 2;
            // 当前排只能坐四人
            else if ((stat | LEFT) == LEFT || (stat | RIGHT) == RIGHT || (stat | MIDDLE) == MIDDLE) res++;
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：21ms，在所有java提交中击败了86.00%的用户。

内存消耗：47.2MB，在所有java提交中击败了82.93%的用户。

## 官方解题

&emsp;上述思路参考官方解题。