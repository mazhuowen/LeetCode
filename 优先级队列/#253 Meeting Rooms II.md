[toc]

Given an array of meeting time intervals consisting of start and end times `[[s1,e1],[s2,e2],...]` $(s_i < e_i)$, find the minimum number of conference rooms required.



**Note**: input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.



## 题目解读

&emsp;给定一组数组代表会议开始时间、结束时间，得出需要的会议室数目。

```java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        
    }
}
```

## 程序设计

* 首先会议开始时间最早的会议会占用会议室，后续会议要根据当前会议结束时间来判断。即检查最早结束会议的会议室，检查开始时间是否大于结束时间，是则可以共用这件会议室，否则只能新开会议室。
* 实质是根据开始时间排序会议，根据结束时间入队，如果当前入队元素开始时间大于堆顶最早结束时间，则可以共用会议室，否则新开会议室。

```java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        // 根据会议开始时间排序
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        PriorityQueue<Integer> record = new PriorityQueue<>();
        for(int i = 0; i < intervals.length; i++) {
            int[] cur = intervals[i];
            // 当前会议开始时间在最早结束会议的时间之后，可以共用一间会议室，出队当前会议结束时间
            if(!record.isEmpty() && cur[0] >= record.peek()) {
                record.poll();
            }
            // 共用会议室更新会议结束时间或没法共用新开一间会议室，入队结束时间
            record.add(cur[1]);
        }
        // 返回堆容量
        return record.size();
    }
}
```

## 性能分析

&emsp;排序花费$O(N\log_2N)$，入队花费$O(N\log_2N)$，总的时间复杂度为$O(N\log_2N)$；空间复杂度为$O(N)$。

执行用时：12ms，在所有java提交中击败了46.79%的用户。

内存消耗：48.6MB，在所有java提交中击败了5.04%的用户。

## 官方解题

&emsp;官方还提供了一种算法，分别处理开始时间和结束时间。将两个属性分离并分别处理，这样做其实是可取的，因为当遇到会议结束事件时，意味着一些较早开始的会议已经结束。并不关心到底是哪个会议结束，所需要的只是一些会议结束，从而提供一个空房间。

```java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        int[] start = new int[intervals.length];
        int[] end = new int[intervals.length];
        for(int i = 0; i < intervals.length; i++) {
            start[i] = intervals[i][0];
            end[i] = intervals[i][1];
        }
        Arrays.sort(start);
        Arrays.sort(end);
        int room = 0;
        int startIndex = 0, endIndex = 0;
        while(startIndex < start.length) {
            // 会议室可复用
            if(start[startIndex++] >= end[endIndex]) {
                endIndex++;
            } 
            // 不可复用，会议室加一
            else {
                room++;
            }
        }
        return room;
    }
}
```

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了99.86%的用户。

内存消耗：49.7MB，在所有java提交中击败了5.04%的用户。