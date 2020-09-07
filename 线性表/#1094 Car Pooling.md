[toc]

You are driving a vehicle that has **capacity** empty seats initially available for passengers.  The vehicle **only** drives east (ie. it **cannot** turn around and drive west.)

Given a list of `trips`, `trip[i] = [num_passengers, start_location, end_location]` contains information about the `i`-th trip: the number of passengers that must be picked up, and the locations to pick them up and drop them off.  The locations are given as the number of kilometers due east from your vehicle's initial location.

Return `true` if and only if it is possible to pick up and drop off all passengers for all the given trips. 



**Constraints**:

* $\text{trips.length} \le 1000$
* $\text{trips[i].length} == 3$
* $1 \le \text{trips[i][0]} \le 100$
* $0 \le \text{trips[i][1]} < \text{trips[i][2]} \le 1000$
* $1 \le \text{capacity} \le 100000$



## 题目解读

&emsp;给定旅客站下车位置和人数，求车辆是否可将沿路所有旅客载到下车位置。

```java
class Solution {
    public boolean carPooling(int[][] trips, int capacity) {

    }
}
```

## 程序设计

* 实际上问题转变为车的容量是否大于当前车上总人数的问题。观测到人数只有在上下车的时候发生变化，可以首先根据行程数组得到每个位置的上下车变化人数，初始化车上总人数为$0$，则后续每个位置人数就是当前总人数加上变化人数即可。

```java
class Solution {
    public boolean carPooling(int[][] trips, int capacity) {
        if (trips == null || trips.length == 0) return true;
        // 记录路径上上下车人流变化
        int[] path = new int[1001];
        for (int[] trip : trips) {
            path[trip[1]] += trip[0];
            path[trip[2]] -= trip[0];
        }

        // 采用total叠加每一站的上下车人数得到实时的总人数
        int total = 0;
        for (int i = 0; i < path.length; i++) {
            total += path[i];
            if (total > capacity) return false;
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.1MB，在所有java提交中击败了98.80%的用户。

## 官方解题

&emsp;暂无，密切关注。