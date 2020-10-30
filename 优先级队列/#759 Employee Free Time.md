[toc]

We are given a list `schedule` of employees, which represents the working time for each employee.

Each employee has a list of non-overlapping `Intervals`, and these intervals are in sorted order.

Return the list of finite intervals representing **common, positive-length free time** for all employees, also in sorted order.

(Even though we are representing `Intervals` in the form `[x, y]`, the objects inside are `Intervals`, not lists or arrays. For example, `schedule[0][0].start = 1`, `schedule[0][0].end = 2`, and `schedule[0][0][0]` is not defined).  Also, we wouldn't include intervals like `[5, 5]` in our answer, as they have zero length.



**Constraints**:

* $1 \le \text{schedule.length , schedule[i].length} \le 50$
* $0 \le \text{schedule[i].start} < \text{schedule[i].end} \le 10^8$



## 题目解读

&emsp;给定员工的日程表，表示每个员工的工作时间。每个员工都有一个非重叠的时间段区间，这些时间段已经排好序。要去返回表示所有 员工共同的，正数长度的空闲时间的有限时间段的列表，同样需要排好序。

```java
/*
// Definition for an Interval.
class Interval {
    public int start;
    public int end;

    public Interval() {}

    public Interval(int _start, int _end) {
        start = _start;
        end = _end;
    }
};
*/
class Solution {
    public List<Interval> employeeFreeTime(List<List<Interval>> schedule) {
        
    }
}
```

## 程序设计

* 参考官方第一个解法，观察示例，我们实质上需要区分嵌套重叠的区间，以`(1,3)`、`(2,4)`为例，如果用开括号代表开始时间，闭括号代表结束时间，则这两段时间可以表示为`(())`，不管开括号和哪个闭括号匹配，对于一段重叠或嵌套的时间都可以表示为同等数目的开括号（起始时间）和闭括号（结束时间）。
* 有了这个思路可以将起始时间和结束时间标记，然后排序，根据上面的思路统计出最大区间，则空闲区间就是当前最大区间结束和下一个区间的开始。
* 考虑特殊情况，即开始时间和结束时间相同，此时这两段时间是连起来的，在排序时必须要让结束时间（闭括号）排列到起始时间（开括号）前面。

```java
class Solution {
    public List<Interval> employeeFreeTime(List<List<Interval>> schedule) {
        List<int[]> interval = new LinkedList<>();
        // 将开始时间、结束时间加入链表
        for(List<Interval> times : schedule) {
            for(Interval time : times) {
                // 结束时间标记为1，起始时间标记为-1，当时间相等，结束时间在前面
                interval.add(new int[]{time.start, -1});
                interval.add(new int[]{time.end, 1});
            }
        }
        // 排序
        Collections.sort(interval, (a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
        List<Interval> res = new LinkedList<>();
        // 模拟栈，记录括号数
        int count = 0;
        // 记录前一个时间，-1标识开始
        int preTime = -1;
        for(int[] time : interval) {
            // 上一个时间区间结束，也就是说上一个时间区间的结束值就是空闲区间的开始值
            if(count == 0 && preTime >= 0) {
                // 加入空闲时间
                res.add(new Interval(preTime, time[0]));
            }
            // 更新
            preTime = time[0];
            count += time[1];
        }
        return res;
    }
}
```

* 有了上述思路，可以使用堆来实现，最小堆根据开始时间排序，每次根据堆顶元素判断更新结束时间，直到没有符合要求的元素，这表示上一段连续区间已结束，该结束时间就是空闲区间的开始时间。

```java
class Solution {
    public List<Interval> employeeFreeTime(List<List<Interval>> schedule) {
        // 最小堆，根据开始时间排序
        PriorityQueue<Interval> queue = new PriorityQueue<>((a, b) -> a.start - b.start);
        // 入队
        schedule.forEach(a -> queue.addAll(a));

        List<Interval> res = new LinkedList<>();
        while(!queue.isEmpty()) {
            // 遍历所有重叠、嵌套、紧随时间区间
            Interval cur = queue.poll();
            // 记录最大结束时间（也就是空闲区间的开始时间）
            int maxEnd = cur.end;
            // 更新最大结束时间
            while(!queue.isEmpty() && queue.peek().start <= maxEnd) {
                maxEnd = Math.max(maxEnd, queue.poll().end);
            }
            // 空闲区间的结束时间为下一个时间段的开始时间
            if(!queue.isEmpty()) {
                res.add(new Interval(maxEnd, queue.peek().start));
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;常规方法时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$，其中$N$为所有区间数。

执行用时：25ms，在所有java提交中击败了42.65%的用户。

内存消耗：42.7MB，在所有java提交中击败了95.00%的用户。

&emsp;使用堆的方法时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：14ms，在所有java提交中击败了86.76%的用户。

内存消耗：43.5MB，在所有java提交中击败了95.00%的用户。

## 代码优化

&emsp;使用堆的方法可以进一步精简代码：

```java
class Solution {
    public List<Interval> employeeFreeTime(List<List<Interval>> schedule) {
        // 最小堆，根据开始时间排序
        PriorityQueue<Interval> queue = new PriorityQueue<>((a, b) -> a.start - b.start);
        // 入队
        schedule.forEach(a -> queue.addAll(a));

        List<Interval> res = new LinkedList<>();

        Interval temp = queue.poll();
        while(!queue.isEmpty()) {
             Interval cur = queue.poll();
            // 遍历所有重叠、嵌套、紧随时间区间
            if(cur.start <= temp.end) {
                // 更新结束时间
                if(cur.end > temp.end) {
                    temp = cur;
                }
            } else {
                res.add(new Interval(temp.end, cur.start));
                temp = cur;
            }
        }
        return res;
    }
}
```

## 官方解题

&emsp;官方采用了堆扩展的方式。