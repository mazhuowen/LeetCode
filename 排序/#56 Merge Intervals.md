[toc]

Given a collection of intervals, merge all overlapping intervals.



## 题目解读

&emsp;给定区间，合并有覆盖的区间并返回。

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        
    }
}
```

## 程序设计

* 首先想到的是利用优先级队列，根据起始区间排序并合并区间。

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        if(intervals == null || intervals.length == 0 || intervals[0].length != 2) {
            return intervals;
        }
        List<int[]> l = new ArrayList<>();
        // 最小堆根据开始区间排序
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        for(int[] interval : intervals) {
            queue.add(interval);
        }
        
        while(!queue.isEmpty()) {
            int[] cur = queue.poll();
            // 如果存在交叉则合并，并更新当前区间的范围
            while(!queue.isEmpty() && queue.peek()[0] <= cur[1]) {
                cur[1] = Math.max(cur[1], queue.poll()[1]);
            }
            // 合并完成，加入结果
            l.add(cur);
        }
        // 转化为数组
        int[][] res =  new int[l.size()][];
        for(int i = 0; i < res.length; i++) {
            res[i] = l.get(i);
        }
        return res;
    }
}
```

* 上述采用优先级队列，实质上是对起始区间排序然后遍历合并，可以直接排序然后遍历。

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        if(intervals == null || intervals.length == 0 || intervals[0].length != 2) {
            return intervals;
        }
        // 根据开始时间排序
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        List<int[]> temp = new ArrayList<>();
        int[] cur = intervals[0];
        // 遍历合并
        for(int[] interval : intervals) {
            // 与cur有交集
            if(interval[0] <= cur[1]) {
                // 更新结束区间
                cur[1] = Math.max(cur[1], interval[1]);
            }
            // 无交集，加入cur并重新赋值
            else {
                temp.add(cur);
                cur = interval;
            }
        }
        // 不能遗漏最后一个cur
        temp.add(cur);
        // 拼装
        int[][] res = new int[temp.size()][];
        for(int i = 0; i < temp.size(); i++) {
            res[i] = temp.get(i);
        }
        return res;
    }
}
```

## 性能分析

&emsp;优先级队列时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：5ms，在所有java提交中击败了91.32%的用户。

内存消耗：42.1MB，在所有java提交中击败了53.33%的用户。

&emsp;快速排序时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：7ms，在所有java提交中击败了86.22%的用户。

内存消耗：42.2MB，在所有java提交中击败了50.93%的用户。

## 官方解题

&emsp;同上。