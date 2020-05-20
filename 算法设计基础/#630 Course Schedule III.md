[toc]

There are `n` different online courses numbered from `1` to `n`. Each course has some duration(course length) `t` and closed on `dth` day. A course should be taken continuously for `t` days and must be finished before or on the `dth` day. You will start at the `1st` day.

Given `n` online courses represented by pairs `(t,d)`, your task is to find the maximal number of courses that can be taken.



Note:

* The integer $1 \le d, t, n \le 10000$.
* You can't take two courses simultaneously.



## 题目解读

&emsp;获取最大可参加的课程数，其中`(t,d)`中`t`表示课程持续的天数，`d`表示课程结束的最晚时间。

```java
class Solution {
    public int scheduleCourse(int[][] courses) {

    }
}
```

## 程序设计

* 参考官方思路，首先如果两门课$(t_1, d_1)$和$(t_2, d_2)$满足$d_1 \le d_2$，即后者的结束时间不晚于前者，先学习前者再学习后者总是最优的。因为如果先学习前者，即需要满足$x + t_1 \le d_1$且$x + t_1 + t_2 \le d_2$；如果先学习后者，则需要满足$x + t_2 \le d_2$且$x + t_1 + t_2 \le d_1$。如果后者中的$x + t_1 + t_2 \le d_1$成立，那么显然有$x + t_1 \le d_1$且$x + t_1 + t_2 \le d_1 \le d_2$，即前者一定成立；反之前者成立后者不一定成立。因此先学习前者再学习后者总是最优的。

* 假设在前$i$门课中最多可以选取$k$门课，并且这$k$门课的总时长最短（称为最优方案），那么有下面的不等式：
  $$
  t_1 + t_2 \le d_2\\
  t_1 + t_2 + t_3 \le d_3\\
  \dots\\
  t_1 + t_2 + \dots + t_k \le d_k
  $$

  此时需要判断第$i + 1$门课$(t_0, d_0)$是否可选。如果选取的$k$门课的总时长与$t_0$之和小于等于$d_0$，即$t_1 + t_2 + \dots + t_k + t_0 \le d_0$，那么$(t_0, d_0)$一定可选，可以使用反证法来证明。

* 如果上述不等式不满足，那么找出$t_1, t_2, \dots, t_k$中时间最长的那一门课$t_j$，如果$t_j > t_0$，那么可以将$t_j$用$t_0$代替，即$t_1, t_2, \dots, t_{j-1}, t_{j+1}, \dots, t_k, t_0$是一种最优方案。

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

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：42ms，在所有java提交中击败了76.25%的用户。

内存消耗：48.7MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;上述思路参考官方解题。

