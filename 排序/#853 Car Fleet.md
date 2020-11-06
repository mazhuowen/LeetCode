[toc]

$N$ cars are going to the same destination along a one lane road.  The destination is `target` miles away.

Each car `i` has a constant speed `speed[i]` (in miles per hour), and initial position `position[i]` miles towards the target along the road.

A car can never pass another car ahead of it, but it can catch up to it, and drive bumper to bumper at the same speed.

The distance between these two cars is ignored - they are assumed to have the same position.

A car fleet is some non-empty set of cars driving at the same position and same speed.  Note that a single car is also a car fleet.

If a car catches up to a car fleet right at the destination point, it will still be considered as one car fleet.


How many car fleets will arrive at the destination?



**Note**:

* $0 \le N \le 10^4$
* $0 < \text{target} \le 10^6$
* $0 < \text{speed[i]} \le 10^6$
* $0 \le \text{position[i]} < \text{target}$
* All initial positions are different.



## 题目解读

&emsp;给定车辆的当前位置和速度，后一辆车不能超过前一辆车，给定目的地距离，求最后车辆分几批到达终点。题目限定车辆位置各不相同。

```java
class Solution {
    public int carFleet(int target, int[] position, int[] speed) {

    }
}
```

## 程序设计

* 由于位置是唯一的，且后面的车辆不能超过前面的车辆，首先根据初始位置排序，然后计算每个车辆到达终点的时间。如果在后面的车到达终点的时间小于在前面的车，说明速度比前面的车快，但是根据题意后面的车不能超过前面的车，也就是说这两辆车是一组。
* 后面的车追上前面的车，需要更新到达终点的时间为前面的车的时间。分组初始化为车辆数，依次比较，每遇到追上前面车的，分组计数减一。

```java
class Solution {
    public int carFleet(int target, int[] position, int[] speed) {
        if(position == null || position.length == 0) {
            return 0;
        }
        Car[] cars = new Car[position.length];
        // 记录位置和最终时间
        for(int i = 0; i < position.length; i++) {
            cars[i] = new Car(position[i], (double)(target - position[i]) / speed[i]);
        }
        // 根据位置排序
        Arrays.sort(cars, (a, b) -> b.position - a.position);
        int count = position.length;
        for(int i = 1; i < position.length; i++) {
            // 位置在后面的车不能超过前面的车，即时间不能短于前面的车
            if(cars[i].time <= cars[i - 1].time) {
                // 更新时间为前面的车，与前面的车一组，分组减一
                cars[i].time = cars[i - 1].time;
                count--;
            }
        }
        return count;
    }
}

class Car {
    int position;
    double time;

    Car(int position, double time) {
        this.position = position;
        this.time = time;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：19ms，在所有java提交中击败了88.79%的用户。

内存消耗：42.5MB，在所有java提交中击败了5.43%的用户。

## 官方解题

&emsp;思路同上。