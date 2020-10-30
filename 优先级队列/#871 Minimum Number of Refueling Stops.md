[toc]

A car travels from a starting position to a destination which is `target` miles east of the starting position.

Along the way, there are gas stations.  Each `station[i]` represents a gas station that is `station[i][0]` miles east of the starting position, and has `station[i][1]` liters of gas.

The car starts with an infinite tank of gas, which initially has `startFuel` liters of fuel in it.  It uses 1 liter of gas per 1 mile that it drives.

When the car reaches a gas station, it may stop and refuel, transferring all the gas from the station into the car.

What is the least number of refueling stops the car must make in order to reach its destination?  If it cannot reach the destination, return `-1`.

Note that if the car reaches a gas station with 0 fuel left, the car can still refuel there.  If the car reaches the destination with 0 fuel left, it is still considered to have arrived.



**Note**:

* $1 \le \text{target, startFuel, stations[i][1]} \le 10^9$
* $0 \le \text{stations.length} \le 500$
* $0 < \text{stations[0][0]} < \text{stations[1][0]} < \cdots < \text{stations[stations.length-1][0]} < \text{target}$



## 题目解读

&emsp;求到达目的地停留石油站次数的最少值。

```java
class Solution {
    public int minRefuelStops(int target, int startFuel, int[][] stations) {

    }
}
```

## 程序设计

* 最基本的思路是暴力回溯，最坏情况时间复杂度为$O(2^N)$，会超时。

```java
class Solution {
    int minCount = Integer.MAX_VALUE;

    public int minRefuelStops(int target, int startFuel, int[][] stations) {
        minRefuelStops(stations, -1, target, startFuel, 0);
        return minCount == Integer.MAX_VALUE ? -1 : minCount;
    }

    private void minRefuelStops(int[][] stations, int start, int target, int fuel, int count) {
        if (start >= stations.length) return;

        // 之前走过的行程
        int preDis = 0;
        if (start != -1) preDis = stations[start][0];

        // 当前的燃料可到达终点
        if (fuel >= target - preDis) {
            minCount = Math.min(minCount, count);
            return;
        }
        
        // 尝试到达后续的加油站i加油
        for (int i = start + 1; i < stations.length; i++) {
            int cost = stations[i][0] - preDis;
            // 到达不了
            if (fuel < cost) break;
            // 尝试
            minRefuelStops(stations, i, target, fuel - cost + stations[i][1], count + 1);
        }
    }
}
```

* 对回溯作记忆化，当target较大时，记忆数组较大，导致超出内存限制。

```java
class Solution {
    Integer[][] record;

    public int minRefuelStops(int target, int startFuel, int[][] stations) {
        // 避免值太大导致内存溢出
        if (startFuel >= target) return 0;
        this.record = new Integer[stations.length + 1][target];

        return minRefuelStops(stations, -1, target, startFuel);
    }

    private int minRefuelStops(int[][] stations, int start, int target, int fuel) {
        if (start >= stations.length) return -1;

        // 之前走过的行程
        int preDis = 0;
        if (start != -1) preDis = stations[start][0];

        // 当前的燃料可到达终点
        if (fuel >= target - preDis) return 0;
        System.out.println(start + "==" + fuel + "==" + (target - preDis));

        if (record[start + 1][fuel] != null) return record[start + 1][fuel];
        
        int count = Integer.MAX_VALUE;
        // 尝试到达后续的加油站i加油
        for (int i = start + 1; i < stations.length; i++) {
            int cost = stations[i][0] - preDis;
            // 到达不了
            if (fuel < cost) break;
            // 尝试
            int c = minRefuelStops(stations, i, target, fuel - cost + stations[i][1]);
            if (c != -1) count = Math.min(count, c + 1);
        }
        if (count == Integer.MAX_VALUE) count = -1;
        record[start + 1][fuel] = count;
        return count;
    }
}
```

* 参考官方思路，使用`dp(i)`表示加油`i`次可到达的最大距离。

```java
class Solution {
    public int minRefuelStops(int target, int startFuel, int[][] stations) {
        long[] dp = new long[stations.length + 1];
        dp[0] = startFuel;
        // 当前为i加油站
        for (int i = 0; i < stations.length; i++) {
            // 加油j+1次
            for (int j = i; j >= 0; j--) {
                // 能够到达，则更新
                if (dp[j] >= stations[i][0]) {
                    dp[j + 1] = Math.max(dp[j + 1], dp[j] + stations[i][1]);
                }
            }
        }
        for (int i = 0; i <= stations.length; i++) {
            if (dp[i] >= target) return i;
        }
        return -1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：15ms，在所有java提交中击败了22.49%的用户。

内存消耗：39.5MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方提供了最优解，使用堆的思路。从宏观看，到达消耗的石油为`target`，而添加的石油是初始石油和中途加油站添加之和；可以使用贪心思路，如果当前油足够到达当前石油站，先不加油，记录下，尝试不停留走的更远；如果不可达下一个石油站或目标点，则选择停留在前面某个石油站添加；由上述分析可知必然停留在经过的最大油量的石油站；采用堆来记录经过的石油站的石油量。

```java
class Solution {
    public int minRefuelStops(int target, int startFuel, int[][] stations) {
        // 最大堆
        PriorityQueue<Integer> queue = new PriorityQueue<>((a, b) -> b - a);
        int count = 0, prev = 0;
        for (int[] station : stations) {
            // 到达当前加油站剩余的油
            startFuel -= station[0] - prev;
            // 石油为负数，不可到达，则从前面的加油站选择最大的油量直到可达
            while (!queue.isEmpty() && startFuel < 0) {
                startFuel += queue.poll();
                count++;
            }
            // 不可达
            if (startFuel < 0) return -1;
            // 可达，加入当前石油站油量
            queue.add(station[1]);
            prev = station[0];
        }
    
        startFuel -= target - prev;
        // 石油为负数，不可到达，则从前面的加油站选择最大的油量直到可达
        while (!queue.isEmpty() && startFuel < 0) {
            startFuel += queue.poll();
            count++;
        }
        // 不可达
        return startFuel < 0 ? -1 : count;
    }
}
```

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：4ms，在所有java提交中击败了86.55%的用户。

内存消耗：39.7MB，在所有java提交中击败了100.00%的用户。