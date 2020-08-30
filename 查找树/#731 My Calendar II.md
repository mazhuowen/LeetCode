[toc]

Implement a `MyCalendarTwo` class to store your events. A new event can be added if adding the event will not cause a **triple** booking.

Your class will have one method, `book(int start, int end)`. Formally, this represents a booking on the half open interval `[start, end)`, the range of real numbers x such that $\text{start} \le x < \text{end}$.

A triple booking happens when **three** events have some non-empty intersection (ie., there is some time that is common to all 3 events.)

For each call to the method `MyCalendar.book`, return `true` if the event can be added to the calendar successfully without causing a triple booking. Otherwise, return `false` and do not add the event to the calendar.

Your class will be called like this: `MyCalendar cal = new MyCalendar(); MyCalendar.book(start, end)`



**Note**:

* The number of calls to `MyCalendar.book` per test case will be at most $1000$.
* In calls to `MyCalendar.book(start, end)`, `start` and `end` are integers in the range $[0, 10^9]$.



## 题目解读

&emsp;设计类，使得区间订阅重合次数不能超过三次。

```java
class MyCalendarTwo {

    public MyCalendarTwo() {

    }
    
    public boolean book(int start, int end) {

    }
}

/**
 * Your MyCalendarTwo object will be instantiated and called as such:
 * MyCalendarTwo obj = new MyCalendarTwo();
 * boolean param_1 = obj.book(start,end);
 */
```

## 程序设计

* 使用红黑树记录区间的次数

```java
class MyCalendarTwo {
    TreeMap<Interval, Integer> intervals;

    public MyCalendarTwo() {
        intervals = new TreeMap<>();
    }
    
    public boolean book(int start, int end) {
        Set<Interval> remove = new HashSet<>();
        Set<Interval> addOne = new HashSet<>();
        Set<Interval> addTwo = new HashSet<>();
        boolean complete = false;
        // 遍历start之后的区间
        for (Map.Entry<Interval, Integer> entry : intervals.tailMap(new Interval(0, start + 1)).entrySet()) {
            Interval cur = entry.getKey();
            int count = entry.getValue();
            // 不相交
            if (cur.left >= end) break;
            
            // 相交且次数已经是2次
            if (count == 2) return false;
            else {
                remove.add(cur);
                if (cur.left < start) {
                    // 前后相交
                    if (cur.right < end) {
                        addOne.add(new Interval(cur.left, start));
                        addTwo.add(new Interval(start, cur.right));
                        start = cur.right;
                    } 
                    // 旧区间包含新区间
                    else {
                        addOne.add(new Interval(cur.left, start));
                        addOne.add(new Interval(end, cur.right));
                        addTwo.add(new Interval(start, end));
                        complete = true;
                    }
                } else {
                    if (cur.right < end) {
                        addOne.add(new Interval(start, cur.left));
                        addTwo.add(new Interval(cur.left, cur.right));
                        start = cur.right;
                    } 
                    // 新区间包含旧区间
                    else {
                        addOne.add(new Interval(start, cur.left));
                        addOne.add(new Interval(end, cur.right));
                        addTwo.add(new Interval(cur.left, end));
                        complete = true;
                    }
                }
            }
        }
        if (!complete) addOne.add(new Interval(start, end));

        // 删除老区间，加入新区间
        for (Interval i : remove) intervals.remove(i);
        for (Interval i : addOne) intervals.put(i, 1);
        for (Interval i : addTwo) intervals.put(i, 2);
        return true;
    }
}

class Interval implements Comparable<Interval> {
    int left;
    int right;

    Interval(int left, int right) {
        this.left = left;
        this.right = right;
    }

    @Override
    public int compareTo(Interval that) {
        return this.right == that.right ? this.left - that.left : this.right - that.right;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：47ms，在所有java提交中击败了95.91%的用户。

内存消耗：40.8MB，在所有java提交中击败了23.73%的用户。

## 官方解题

&emsp;官方提供了红黑树的另一种实现，即开括号计数使用负数，闭括号使用正数。