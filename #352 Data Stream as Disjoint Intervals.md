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

&emsp;

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

* 

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

&emsp;



## 官方解题

&emsp;