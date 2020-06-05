[toc]

Given a data stream input of non-negative integers $a_1, a_2, \cdots, a_n, \cdots$, summarize the numbers seen so far as a list of disjoint intervals.

For example, suppose the integers from the data stream are 1, 3, 7, 2, 6, ..., then the summary will be:

```
[1, 1]
[1, 1], [3, 3]
[1, 1], [3, 3], [7, 7]
[1, 3], [7, 7]
[1, 3], [6, 7]
```



**Follow up**:

What if there are lots of merges and the number of disjoint intervals are small compared to the data stream's size?



## 题目解读

&emsp;设计数据结构可以将数据流转化为不相交区间。

```java
class SummaryRanges {

    /** Initialize your data structure here. */
    public SummaryRanges() {
        
    }
    
    public void addNum(int val) {
        
    }
    
    public int[][] getIntervals() {
        
    }
}

/**
 * Your SummaryRanges object will be instantiated and called as such:
 * SummaryRanges obj = new SummaryRanges();
 * obj.addNum(val);
 * int[][] param_2 = obj.getIntervals();
 */
```

## 程序设计

* 最基本的思路是使用红黑树保存已存在的区间，每次插入新值都判断是否有区间前后接壤，有则合并，无则插入。

```java
class SummaryRanges {
    TreeMap<Integer, Integer> start2end;
    TreeMap<Integer, Integer> end2start;

    /** Initialize your data structure here. */
    public SummaryRanges() {
        this.start2end = new TreeMap<>();
        this.end2start = new TreeMap<>();
    }
    
    public void addNum(int val) {
        // 带插入值包含在区间内
        Integer prevStart = start2end.floorKey(val);
        if (prevStart != null && start2end.get(prevStart) >= val) return;

        boolean mergeFlag = false;
        // 合并后继区间
        Integer nextStart = start2end.ceilingKey(val);
        if (nextStart != null && val + 1 == nextStart) {
            mergeFlag = true;
            mergeInterval(val, nextStart);
        }
        // 合并前驱区间
        Integer prevEnd = end2start.floorKey(val);
        if (prevEnd != null && val - 1 == prevEnd) {
            mergeFlag = true;
            mergeInterval(end2start.get(prevEnd), val);
        }
        // 没有接壤的区间，插入当前值
        if (!mergeFlag) addInterval(val, val);
    }
    
    public int[][] getIntervals() {
        int[][] res = new int[start2end.size()][2];
        int idx = 0;
        for (Map.Entry<Integer, Integer> entry : start2end.entrySet()) {
            res[idx++] = new int[]{entry.getKey(), entry.getValue()};
        }
        return res;
    }

    // 合并区间
    private void mergeInterval(int newStart, int oldStart) {
        int newEnd = start2end.getOrDefault(oldStart, oldStart);
        removeInterval(newStart);
        removeInterval(oldStart);
        addInterval(newStart, newEnd);
    }

    private void removeInterval(int s) {
        Integer e = start2end.remove(s);
        if (e != null) end2start.remove(e);
    }

    private void addInterval(int s, int e) {
        start2end.put(s, e);
        end2start.put(e, s);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(N)$。

执行用时：91ms，在所有java提交中击败了53.88%的用户。

内存消耗：44.7MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区还有思路采用二分查找在链表中查找前后区间，然后判断是否插入、合并。

```java
class SummaryRanges {

    List<int[]> intervals;

    /** Initialize your data structure here. */
    public SummaryRanges() {
        this.intervals = new LinkedList<>();
    }
    
    public void addNum(int val) {
        if (intervals.isEmpty()) {
            intervals.add(new int[]{val, val});
            return;
        }

        // 二分查找当前值后面最接近的区间
        int idx = binarySearch(val);

        // 已包含在区间内，直接返回
        if (idx != 0 && intervals.get(idx - 1)[1] >= val) return;
        
        boolean mergeFlag = false;
        // 与前面接壤，合并
        if (idx != 0 && intervals.get(idx - 1)[1] == val - 1) {
            mergeFlag = true;
            intervals.get(idx - 1)[1] = val;
        }
        // 与后面接壤
        if (idx < intervals.size() && intervals.get(idx)[0] == val + 1) {
            // 前面合并过，需要合并两个区间
            if (mergeFlag) {
                intervals.get(idx - 1)[1] = intervals.get(idx)[1];
                intervals.remove(idx);
            } else {
                intervals.get(idx)[0] = val;
            }
            mergeFlag = true;
        }

        // 未合并过，加入区间
        if (!mergeFlag) {
            intervals.add(idx, new int[]{val, val});
        }
    }
    
    public int[][] getIntervals() {
        return intervals.toArray(new int[0][2]);
    }

    // 查找大于target的最小起始时间索引
    private int binarySearch(int target) {
        int left = 0, right = intervals.size();
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (intervals.get(mid)[0] > target) right = mid;
            else left = mid + 1;
        }
        return left;
    }
}
```

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(N)$。

执行用时：84ms，在所有java提交中击败了91.75%的用户。

内存消耗：44.2MB，在所有java提交中击败了100.00%的用户。