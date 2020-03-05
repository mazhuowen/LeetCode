[toc]

Given a set of non-overlapping intervals, insert a new interval into the intervals (merge if necessary).

You may assume that the intervals were initially sorted according to their start times.



## 题目解读

&emsp;插入一个区间到不交叉区间，返回新的区间。

```java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        
    }
}
```

## 程序设计

* 初步的思路是先二分查找找到插入点，再合并判断。但是最后还是要遍历加入之前的元素，不如采用贪婪法，直接从头遍历，比较合并。

```java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        if(intervals == null || intervals.length == 0) {
            return new int[][]{newInterval};
        } else if(newInterval == null || newInterval.length == 0) {
            return intervals;
        }
        List<int[]> res = new ArrayList<>();
        int idx = 0;
        // 前半部分无交集区间
        while(idx < intervals.length && intervals[idx][1] < newInterval[0]) {
            // 直接加入
            res.add(intervals[idx++]);
        }
        // 更新起始区间（当前区间与新插入区间相交或当前区间包含新插入区间，更新起始区间）
        if(idx < intervals.length && intervals[idx][0] < newInterval[0]) {
            newInterval[0] = intervals[idx][0];
        }
        // 向后有交集，更新结束区间
        while(idx < intervals.length && intervals[idx][0] <= newInterval[1]) {
            newInterval[1] = Math.max(newInterval[1], intervals[idx++][1]);
        }
        // 更新完毕，加入链表
        res.add(newInterval);
        // 加入剩余元素
        while(idx < intervals.length) {
            res.add(intervals[idx++]);
        }
        return res.toArray(new int[res.size()][2]);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了99.79%的用户。

内存消耗：42.3MB，在所有java提交中击败了22.06%的用户。

## 官方解题

&emsp;同上。