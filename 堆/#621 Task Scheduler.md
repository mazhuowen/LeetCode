[toc]

Given a char array representing tasks CPU need to do. It contains capital letters A to Z where different letters represent different tasks. Tasks could be done without original order. Each task could be done in one interval. For each interval, CPU could finish one task or just be idle.

However, there is a non-negative cooling interval **n** that means between two **same tasks**, there must be at least n intervals that CPU are doing different tasks or just be idle.

You need to return the **least** number of intervals the CPU will take to finish all the given tasks.

Note:

* The number of tasks is in the range [1, 10000].
* The integer n is in the range [0, 100].



## 题目解读

&emsp;给定字符数组表示CPU需要执行的任务列表，其中包含使用大写的A-Z字母表示的26种不同种类的任务。任务可以以任意顺序执行，并且每个任务都可以在1个单位时间内执行完。CPU在任何一个单位时间内都可以执行一个任务，或者在待命状态。需要注意的是两个相同种类的任务之间必须有长度为n的冷却时间，因此至少有连续n个单位时间内CPU在执行不同的任务，或者在待命状态。需要计算完成所有任务所需要的最短时间。

```java
class Solution {
    public int leastInterval(char[] tasks, int n) {
        
    }
}
```

## 程序设计

