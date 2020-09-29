[toc]

Implement the class `UndergroundSystem` that supports three methods:

* `checkIn(int id, string stationName, int t)`

  * A customer with id card equal to `id`, gets in the station `stationName` at time `t`.
  * A customer can only be checked into one place at a time.
* `checkOut(int id, string stationName, int t)`
  * A customer with id card equal to `id`, gets out from the station `stationName` at time `t`.

* `getAverageTime(string startStation, string endStation)` 
  * Returns the average time to travel between the `startStation` and the `endStation`.
  * The average time is computed from all the previous traveling from `startStation` to `endStation` that happened **directly**.
  * Call to `getAverageTime` is always valid.

You can assume all calls to `checkIn` and `checkOut` methods are consistent. That is, if a customer gets in at time $t_1$ at some station, then it gets out at time $t_2$ with $t_2 > t_1$. All events happen in chronological order.



**Constraints**:

* There will be at most $20000$ operations.
* $1 \le id, t \le 10^6$
* All strings consist of uppercase, lowercase English letters and digits.
* $1 \le \text{stationName.length} \le 10$
* Answers within $10^{-5}$ of the actual value will be accepted as correct.



## 题目解读

&emsp;设计地铁系统，可统计任意两个站之间的平均时间。

```java
class UndergroundSystem {

    public UndergroundSystem() {

    }
    
    public void checkIn(int id, String stationName, int t) {

    }
    
    public void checkOut(int id, String stationName, int t) {

    }
    
    public double getAverageTime(String startStation, String endStation) {

    }
}

/**
 * Your UndergroundSystem object will be instantiated and called as such:
 * UndergroundSystem obj = new UndergroundSystem();
 * obj.checkIn(id,stationName,t);
 * obj.checkOut(id,stationName,t);
 * double param_3 = obj.getAverageTime(startStation,endStation);
 */
```

## 程序设计

* 使用哈希表保存入站未结束行程的信息及结束行程的统计信息。

```java
class UndergroundSystem {
    // 在地铁中的乘客的入站时间与站名
    Map<Integer, Integer> startTime;
    Map<Integer, String> startStation;
    // 已搭乘结束的统计
    Map<String, Static> staticSystem;

    public UndergroundSystem() {
        this.startTime = new HashMap<>();
        this.startStation = new HashMap<>();
        this.staticSystem = new HashMap<>();
    }
    
    public void checkIn(int id, String stationName, int t) {
        startTime.put(id, t);
        startStation.put(id, stationName);
    }
    
    public void checkOut(int id, String stationName, int t) {
        if (startTime.get(id) == null) throw new IllegalArgumentException("invalid param");
        int startT = startTime.get(id);
        String startS = startStation.get(id);
        startTime.remove(id);
        startStation.remove(id);

        String key = startS + "-" + stationName;
        if (staticSystem.get(key) == null) staticSystem.put(key, new Static(0, 0));
        staticSystem.get(key).time += t - startT;
        staticSystem.get(key).count++;
    }
    
    public double getAverageTime(String startStation, String endStation) {
        String key = startStation + "-" + endStation;
        if (staticSystem.get(key) == null) throw new IllegalArgumentException("invalid param");
        return (double)staticSystem.get(key).time / staticSystem.get(key).count;
    }
}

class Static {
    // 总的花费时间
    int time;
    // 人次
    int count;

    Static(int time, int count) {
        this.time = time;
        this.count = count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(N)$。

执行用时：123ms，在所有java提交中击败了25.00%的用户。

内存消耗：51.4 MB，在所有java提交中击败了87.91%的用户。

## 官方解题

&emsp;官方思路类似。