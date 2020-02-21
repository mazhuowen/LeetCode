[toc]

Given a set of intervals, for each of the interval `i`, check if there exists an interval `j` whose start point is bigger than or equal to the end point of the interval `i`, which can be called that `j` is on the "right" of `i`.

For any interval `i`, you need to store the minimum interval `j`'s index, which means that the interval `j` has the minimum start point to build the "right" relationship for interval `i`. If the interval `j` doesn't exist, store -1 for the interval `i`. Finally, you need output the stored value of each interval as an array.

Note:

* You may assume the interval's end point is always bigger than its start point.
* You may assume none of these intervals have the same start point.



## 题目解读

&emsp;给定区间集合，找到每个区间的右侧区间，返回索引数组。题目假设每个区间的结束大于开始值，且不同区间不会有相同的开始值。

```java
class Solution {
    public int[] findRightInterval(int[][] intervals) {
        
    }
}
```

## 程序设计

* 最容易想到的是暴力法，两层循环遍历更新当前区间的右区间。

```java
class Solution {
    public int[] findRightInterval(int[][] intervals) {
        int[] res = new int[intervals.length];
        for(int i = 0; i < intervals.length; i++) {
            // 赋值-1
            res[i] = -1;
            for(int j = 0; j < intervals.length; j++) {
                // 更新紧随当前区间的右区间
                if(intervals[j][0] >= intervals[i][1] && 
                (res[i] == -1 || intervals[j][0] < intervals[res[i]][0])) {
                    res[i] = j;
                }
            }
        }
        return res;
    }
}
```

* 可以看到两层循环，里面的循环比较数组其它区间的开始时间和当前数组的结束时间，如果我们可以提前根据开始时间排序，然后二分查找，时间复杂度会更小一些。

```java
class Solution {
    public int[] findRightInterval(int[][] intervals) {
        int[] res = new int[intervals.length];
        // 哈希表记录区间索引关系
        Map<int[], Integer> map = new HashMap<>();
        for(int i = 0; i < intervals.length; i++) {
            map.put(intervals[i], i);
        }
        // 排序
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        // 遍历查找
        for(int i = 0; i < intervals.length; i++) {
            int index = map.get(intervals[i]);
            int[] right = binarySearch(intervals, intervals[i][1], 0, intervals.length - 1);
            // 特殊判断是否存在（对应二分查找最后在len-1的情况）
            if(right[0] < intervals[i][1]) {
                res[index] = -1;
            } else {
                res[index] = map.get(right);
            }
        }
        return res;
    }
    // 返回大于等于target的区间
    private int[] binarySearch(int[][] intervals, int target, int left, int right) {
        while(left < right) {
            int mid = left + (right - left) / 2;
            // right保持大于等于target的边界
            if(intervals[mid][0] >= target) {
                right = mid;
            } 
            // left逼近
            else {
                left = mid + 1;
            }
        }
        // 如果没找到，则此时left与right重合到索引len-1，需要再次判断
        return intervals[left];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：859ms，在所有java提交中击败了5.33%的用户。

内存消耗：45.3MB，在所有java提交中击败了91.43%的用户。

&emsp;排序后二分查找的时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时:26ms，在所有java提交中击败了59.43%的用户。

内存消耗:49MB，在所有java提交中击败了22.14%的用户。

## 官方解题

&emsp;官方给出了一系列从暴力到技巧的方法。在暴力法后官方还提供了优化，即使用哈希表保存区间和索引关系，然后根据区间开始时间排序数组，虽然这样还是需要两层循环，但是内层不必每次从头遍历，不过时间复杂度还是没变。

&emsp;除了这些基础的解法，官方还提供了TreeMap的解法。字典键为开始时间，值为索引。利用有序字典的`ceilingEntry`方法返回下一个大于结束时间的开始时间。

```java
class Solution {
    public int[] findRightInterval(int[][] intervals) {
        int[] res = new int[intervals.length];
        TreeMap<Integer, Integer> map = new TreeMap<>();
        // 以开始时间为键放入有序字典
        for(int i = 0; i < intervals.length; i++) {
            map.put(intervals[i][0], i);
        }
        // 从字典中找到大于结束时间的最小开始时间
        for(int i = 0; i < intervals.length; i++) {
            Map.Entry<Integer, Integer> entry = map.ceilingEntry(intervals[i][1]);
            res[i] = entry == null ? -1 : entry.getValue();
        }
        return res;
    }
}
```

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：25ms，在所有java提交中击败了61.89%的用户。

内存消耗：45MB，在所有java提交中击败了92.86%的用户。

&emsp;官方还提供了根据开始和结束排序，然后类似两个有序数组的比较来遍历并更新。

```java
class Solution {
    public int[] findRightInterval(int[][] intervals) {
        int[] res = new int[intervals.length];
        // 记录区间索引关系
        Map<int[], Integer> map = new HashMap<>();
        for(int i = 0; i < intervals.length; i++) {
            map.put(intervals[i], i);
        }
        // 排序
        int[][] endIntervals = Arrays.copyOf(intervals, intervals.length);
        // 根据结束时间排序
        Arrays.sort(endIntervals, (a, b) -> a[1] - b[1]);
        // 根据开始时间排序
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        // 迭代比较
        int i = 0, j = 0;
        while(i < intervals.length) {
            // 开始时间已经遍历完
            if(j >= intervals.length) {
                res[map.get(endIntervals[i])] = -1;
                i++;
            }
            // 找到
            else if(endIntervals[i][1] <= intervals[j][0]) {
                res[map.get(endIntervals[i])] = map.get(intervals[j]);
                // 结束时间迭代
                i++;
            } else {
                // 开始时间迭代
                j++;
            }
        }
        return res;
    }
}
```

&emsp;时间复杂度、空间复杂度不变。

执行用时：38ms，在所有java提交中击败了40.98%的用户。

内存消耗：48.1MB，在所有java提交中击败了30.00%的用户。