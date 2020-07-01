[toc]

Implement a `MyCalendarThree` class to store your events. A new event can **always** be added.

Your class will have one method, `book(int start, int end)`. Formally, this represents a booking on the half open interval `[start, end)`, the range of real numbers `x` such that $\text{start} \le x < \text{end}$.

A K-booking happens when **K** events have some non-empty intersection (ie., there is some time that is common to all K events.)

For each call to the method `MyCalendar.book`, return an integer **K** representing the largest integer such that there exists a `K`-booking in the calendar.

Your class will be called like this: `MyCalendarThree cal = new MyCalendarThree()`; `MyCalendarThree.book(start, end)`



**Note**:

* The number of calls to `MyCalendarThree.book` per test case will be at most `400`.
* In calls to `MyCalendarThree.book(start, end)`, `start` and `end` are integers in the range $[0, 10^9]$.



## 题目解读

&emsp;当**K**个日程安排有一些时间上的交叉时（例如K个日程安排都在同一时间内），就会产生**K**次预订。求每次预订日程后的最大预订数。

```java
class MyCalendarThree {

    public MyCalendarThree() {

    }
    
    public int book(int start, int end) {

    }
}

/**
 * Your MyCalendarThree object will be instantiated and called as such:
 * MyCalendarThree obj = new MyCalendarThree();
 * int param_1 = obj.book(start,end);
 */
```

## 程序设计

* 采用线段树，每次更新区间的最大订阅次数，需要注意延迟更新的问题；当更新的区间不是叶子区间，则面临更新所有子区间的问题，为了节省时间，可采用延迟更新的方式，只有遍历到该区间再更新。

```java
class MyCalendarThree {
    TreeNode root;

    public MyCalendarThree() {
        this.root = new TreeNode(0, 1_000_000_000);
    }

    public int book(int start, int end) {
        return root.insert(start, end);
    }
}

class TreeNode {
    // 区间
    int start, end;
    // 最大预订次数
    int maxTime;
    // 当前区间延迟更新次数（即子区间未更新）
    int delayTime;
    // 子节点
    TreeNode left, right;

    TreeNode(int start, int end) {
        this.start = start;
        this.end = end;
    }

    private int getMid() {
        return (start + end) / 2;
    }

    private TreeNode left() {
        if (this.left == null) this.left = new TreeNode(start, getMid());
        return this.left;
    }

    private TreeNode right() {
        if (this.right == null) this.right = new TreeNode(getMid(), end);
        return this.right;
    }

    public int insert(int s, int e) {
        // 包含，更新区间
        if (this.start >= s && this.end <= e) {
            this.delayTime++;
            this.maxTime++;
        }
        // 相交
        else if (this.end > s && this.start < e) {
            // 自上向下延迟更新
            this.left().maxTime += this.delayTime;
            this.left().delayTime += this.delayTime;
            this.right().maxTime += this.delayTime;
            this.right().delayTime += this.delayTime;
            // 延迟更新完成，清空
            this.delayTime = 0;

            // 自下向上更新最大次数
            this.maxTime = Math.max(this.maxTime, Math.max(this.left().insert(s, e), this.right().insert(s, e)));
        }

        return this.maxTime;
    }
}
```

## 性能分析

&emsp;每次操作，时间复杂度最坏为$O(\log_2{10^9})$，共计$O(N\log_2{10^9})  \approx O(N)$，空间复杂度最坏为$O(2 \times 10^9 - 1)$。

执行用时：28ms，在所有java提交中击败了95.51%的用户。

内存消耗：41.7MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方提供了红黑树记录区间端点数目，将问题转化为括号匹配中最大括号深度的问题，只需依次从小到大遍历括号，开括号加一，闭括号减一，记录最大

```java
class MyCalendarThree {
    TreeMap<Integer, Integer> record;

    public MyCalendarThree() {
        this.record = new TreeMap<>();
    }
    
    public int book(int start, int end) {
        // 开括号加一
        record.put(start, record.getOrDefault(start, 0) + 1);
        // 闭括号减一
        record.put(end, record.getOrDefault(end, 0) - 1);

        int interval = 0, res = 0;
        for (int val : record.values()) {
            interval += val;
            res = Math.max(res, interval);
        }
        return res;
    }
}
```

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：149ms，在所有java提交中击败了47.19%的用户。

内存消耗：40.3MB，在所有java提交中击败了100.00%的用户。