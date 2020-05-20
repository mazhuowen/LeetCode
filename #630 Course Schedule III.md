[toc]

There are `n` different online courses numbered from `1` to `n`. Each course has some duration(course length) `t` and closed on `dth` day. A course should be taken continuously for `t` days and must be finished before or on the `dth` day. You will start at the `1st` day.

Given `n` online courses represented by pairs `(t,d)`, your task is to find the maximal number of courses that can be taken.



Note:

* The integer $1 \le d, t, n \le 10000$.
* You can't take two courses simultaneously.



## 题目解读

&emsp;获取最大可参加的课程数。

```java
class Solution {
    public int scheduleCourse(int[][] courses) {

    }
}
```

## 程序设计

* 

```java
class Solution {
    public int scheduleCourse(int[][] courses) {
        if (courses == null || courses.length == 0) return 0;

        Arrays.sort(courses, (a, b) -> a[1] - b[1]);

        // 起始时间
        int times = 0;
        // 最大堆
        PriorityQueue<Integer> queue = new PriorityQueue<>((a, b) -> b - a);
        for (int[] course : courses) {
            // 当前课程满足要求
            if (times + course[0] <= course[1]) {
                times += course[0];
                queue.add(course[0]);
            }
            // 当前课程不满足要求，检查时长最长的课是否比当前课长
            else if (!queue.isEmpty() && queue.peek() > course[0]) {
                // 时长较长的课程出队
                times += course[0] - queue.poll();
                queue.add(course[0]);
            }
            // 当前课程不满足要求
        }
        return queue.size();
    }
}
```

## 性能分析

&emsp;



## 官方解题

&emsp;