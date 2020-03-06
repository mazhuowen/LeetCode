[toc]

Given an array of meeting time intervals consisting of start and end times `[[s1,e1],[s2,e2],...]` `(si < ei)`, determine if a person could attend all meetings.



## 题目解读

&emsp;给定会议时间，判断是否可以参加所有的会议。

```java
class Solution {
    public boolean canAttendMeetings(int[][] intervals) {

    }
}
```

## 程序设计

* 根据会议起始时间排序，然后比较相邻的区间结束时间与开始时间是否交叉。

```java
class Solution {
    public boolean canAttendMeetings(int[][] intervals) {
        if(intervals == null || intervals.length < 2) {
            return true;
        }
        // 根据开始时间排序
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        for(int i = 1; i < intervals.length; i++) {
            // 前后区间交叉
            if(intervals[i][0] < intervals[i - 1][1]) {
                return false;
            }
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：6ms，在所有java提交中击败了93.62%的用户。

内存消耗：42.7MB，在所有java提交中击败了5.95%的用户。

## 官方解题

&emsp;同上。