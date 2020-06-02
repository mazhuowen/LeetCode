[toc]

Given a list of intervals, remove all intervals that are covered by another interval in the list. Interval `[a,b)` is covered by interval `[c,d)` if and only if $c \le a$ and $b \le d$.

After doing so, return the number of remaining intervals.



Constraints:

* $1 \le \text{intervals.length} \le 1000$
* $0 \le \text{intervals[i][0]} < \text{intervals[i][1]} \le 10^5$
* $\text{intervals[i]} \ne \text{intervals[j]}$ for all $i \ne j$



## 题目解读

&emsp;求移除被覆盖的区间后剩余的区间数目。

```java
class Solution {
    public int removeCoveredIntervals(int[][] intervals) {

    }
}
```

## 程序设计

* 由于被包含的区间其上下界必然在比它大的区间内，可以根据起始区间排序，然后后面的区间结束值小于前面的区间结束值，说明后面的区间被包含在前面的区间；这样只需记录遍历过的最大的结束值与当前结束值比较即可。
* 需要注意起始区间相同，则结束区间值大的需要排序后在前面。

```java
class Solution {
    public int removeCoveredIntervals(int[][] intervals) {
        if (intervals == null) return 0;
        // 根据起始区间排序，起始相等，则结束区间大的在前面
        Arrays.sort(intervals, (a, b) -> a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]);

        int count = 0;
        int preMaxEnd = Integer.MIN_VALUE;
        for (int[] interval : intervals) {
            if (interval[1] <= preMaxEnd) count++;
            else preMaxEnd = interval[1];
        }
        return intervals.length - count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：7ms，在所有java提交中击败了71.86%的用户。

内存消耗：39.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。