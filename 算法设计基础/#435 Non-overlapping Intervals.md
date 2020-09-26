[toc]

Given a collection of intervals, find the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.



**Note**:

* You may assume the interval's end point is always bigger than its start point.
* Intervals like `[1,2]` and `[2,3]` have borders "touching" but they don't overlap each other.



## 题目解读

&emsp;返回移除的最少区间数，使得剩余的区间不重叠。

```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {

    }
}
```

## 程序设计

* 对于重叠的区间，选择最先结束的区间总是最优的，可采用贪婪法来判断，每次都选择最先结束，排除重叠区间。

```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[1] == b[1] ? b[0] - a[0] : a[1] - b[1]);
        int curEnd = Integer.MIN_VALUE, res = 0;
        for (int[] interval : intervals) {
            // 不重叠
            if (interval[0] >= curEnd) curEnd = interval[1];
            // 重叠
            else res++;
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：4ms，在所有java提交中击败了86.81%的用户。

内存消耗：39.1MB，在所有java提交中击败了16.96%的用户。

## 官方解题

&emsp;官方还提供了动态规划的方法。