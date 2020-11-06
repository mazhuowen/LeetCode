[toc]

Implement a `MyCalendar` class to store your events. A new event can be added if adding the event will not cause a double booking.

Your class will have the method, `book(int start, int end)`. Formally, this represents a booking on the half open interval `[start, end)`, the range of real numbers `x` such that `start <= x < end`.

A double booking happens when two events have some non-empty intersection (ie., there is some time that is common to both events.)

For each call to the method `MyCalendar.book`, return true if the event can be added to the calendar successfully without causing a double booking. Otherwise, return false and do not add the event to the calendar.



**Note**:

* The number of calls to `MyCalendar.book` per test case will be at most $1000$.
* In calls to `MyCalendar.book(start, end)`, `start` and `end` are integers in the range $[0, 10^9]$.



## 题目解读

&emsp;日程管理器。

```java
class MyCalendar {

    public MyCalendar() {

    }
    
    public boolean book(int start, int end) {

    }
}

/**
 * Your MyCalendar object will be instantiated and called as such:
 * MyCalendar obj = new MyCalendar();
 * boolean param_1 = obj.book(start,end);
 */
```

## 程序设计

* 引入红黑树保存区间，键为起始区间，值为结束区间，每次插入，比对前后区间是否交叉。

```java
class MyCalendar {
    TreeMap<Integer, Integer> record;

    public MyCalendar() {
        this.record = new TreeMap<>();
    }
    
    public boolean book(int start, int end) {
        // 不合法
        if (start < 0 || start >= end) return false;

        Map.Entry<Integer, Integer> preInterval = record.floorEntry(start);
        // 前一区间与当前区间交叉
        if (preInterval != null && preInterval.getValue() > start) return false;
        Map.Entry<Integer, Integer> nextInterval = record.ceilingEntry(start);
        // 后一区间与当前区间交叉
        if (nextInterval != null && nextInterval.getKey() < end) return false;
        record.put(start, end);
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(N)$。

执行用时：29ms，在所有java提交中击败了83.68%的用户。

内存消耗：40.4MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。