[toc]

There are $n$ flights, and they are labeled from $1$ to $n$.

We have a list of flight bookings.  The `i`-th booking `bookings[i] = [i, j, k]` means that we booked $k$ seats from flights labeled $i$ to $j$ inclusive.

Return an array answer of length $n$, representing the number of seats booked on each flight in order of their label.



**Constraints**:

* $1 \le \text{bookings.length} \le 20000$
* $1 \le \text{bookings[i][0]} \le \text{bookings[i][1]} \le n \le 20000$
* $1 \le \text{bookings[i][2]} \le 10000$



## 题目解读

&emsp;给定区间及数值，求最后每个坐标得值。

```java
class Solution {
    public int[] corpFlightBookings(int[][] bookings, int n) {

    }
}
```

## 程序设计

* 采用标记两个区间值，然后线性扫描得方式。

```java
class Solution {
    public int[] corpFlightBookings(int[][] bookings, int n) {
        int[] res = new int[n];
        for (int[] booking : bookings) {
            // 整体坐标减一
            int from = booking[0] - 1, to = booking[1];
            res[from] += booking[2];
            if (to < n) res[to] -= booking[2];
        }
        for (int i = 1; i < n; i++) res[i] += res[i - 1];  
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\max(M,N))$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了99.45%的用户。

内存消耗：53.9MB，在所有java提交中击败了40.55%的用户。

## 官方解题

&emsp;暂无，密切关注。