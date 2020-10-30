[toc]

Given an array of `events` where `events[i] = [startDayi, endDayi]`. Every event `i` starts at `startDayi` and ends at `endDayi`.

You can attend an event `i` at any day `d` where $\text{startTime}_i \le d \le \text{endTime}_i$. Notice that you can only attend one event at any time `d`.

Return the maximum number of events you can attend.



**Constraints**:

* $1 \le \text{events.length} \le 10^5$
* $\text{events[i].length} == 2$
* $1 \le \text{events[i][0]} \le \text{events[i][1]} \le 10^5$



## 题目解读

&emsp;给出会议区间时间，每天只能参加一个会议，求可参加的最多的会议数。

```java
class Solution {
    public int maxEvents(int[][] events) {

    }
}
```

## 程序设计

* 首先观察到可以贪心法模拟分配参加会议时间，根据起始时间排序，然后依次分配会议时间，如果分配的时间超过当前区间，则说明无法参加当前会议。
* 对于`[1,5],[1,5],[1,5],[2,3],[2,3]`，如果只根据起始时间排序，则得到的答案是错误的，事实上随着分配的进行，起始区间也要作变动，如第一个区间分配时间为1，则后续可分配时间为2，将小于该值的起始值变为2，得到`[2,3],[2,3],[2,5],[2,5]`，分配2下次待分配为3，变为`[3,3],[3,5],[3,5]`，以此类推，可用优先级队列动态排序。
* 该思路在遇到起始区间相等，结束区间递增的长数组时会超时，每一步都会修改整个堆数据。

```java
class Solution {
    public int maxEvents(int[][] events) {
        if (events == null || events.length == 0) return 0;

        // 最小堆，根据起始时间排序，相等则根据结束时间排序
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
        for (int[] event : events) {
            queue.add(event);
        }

        // 分配的会议时间
        int meetingDay = -1;
        int count = 0;
        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            // 当前会议区段无法参加
            if (meetingDay != -1 && meetingDay > cur[1]) continue;
            // 分配会议时间，会议时间在当前区间前，分配为起始区间
            if (meetingDay < cur[0]) meetingDay = cur[0];
            // 会议时间在当前区间内，加一
            else meetingDay++;
            count++;

            // 修改堆中起始时间（关键）
            while (!queue.isEmpty() && queue.peek()[0] <= meetingDay) {
                int[] temp = queue.poll();
                // 将起始区间缩小并重新加入堆排序
                if (temp[1] > meetingDay) {
                    temp[0] = meetingDay + 1;
                    queue.add(temp);
                }
            }
        }
        return count;
    }
}
```

* 继承上述思路，将堆扩展拆分为两部分：
  * 首先根据会议起始时间排序，若起始时间相等则根据结束时间排序；
  * 如果优先级队列为空，表示之前的会议区间与当前无交集，会议时间需要重新定位到数组当前位置的起始时间，由于当前位置起始时间最小，且在起始时间相等的区间里结束时间最小，是最优解；
  * 如果优先级队列不为空，优先级队列中的区间是起始小于等于已分配会议时间，结束大于已分配时间的区间，只需选择结束时间最小的即可；
  * 每次分配会议时间后，都需要将优先级队列中结束时间小于会议时间的区间去除，同时遍历数组将区间包含会议时间的区间加入进来。

```java
class Solution {
    public int maxEvents(int[][] events) {
        if (events == null) throw new IllegalArgumentException("invalid param");

        // 根据起始时间排序，起始时间相等则根据结束时间排序
        Arrays.sort(events, (a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
        // 根据结束时间排序
        PriorityQueue<Integer> queue = new PriorityQueue<>();

        int count = 0;
        // 数组遍历索引及当前会议分配时间
        int idx = 0, meetingDay = -1;
        while (idx < events.length || !queue.isEmpty()) {
            // 分配起始区间为起始会议时间
            if (queue.isEmpty()) meetingDay = events[idx++][0];
            // 在区间内，分配连续时间
            else {
                queue.poll();
                meetingDay++;
            }
            count++;

            // 出堆结束时间在当前会议时间之前的区间
            while (!queue.isEmpty() && queue.peek() <= meetingDay) queue.poll();
            // 遍历略过在当前会议时间之前的会议
            while (idx < events.length && events[idx][1] <= meetingDay) idx++;
            // 将后续meetingDay在区间内的区间加入堆
            while (idx < events.length && events[idx][0] <= meetingDay + 1 && events[idx][1] > meetingDay) {
                queue.add(events[idx++][1]);
            }
        }

        return count;
    }
}
```

> 测试用例：`[[1,5],[1,5],[1,5],[2,3],[2,3]]`测试基本逻辑运转；`[[1,5],[1,5],[1,5],[2,3],[2,3],[2,3]]`测试会议分配时间超过堆中结束时间的出队操作；`[1,1],[1,2],...[1,n]`测试特殊情况的时间性能。

## 性能分析

&emsp;时间复杂度为$O(T\log_2N)$，空间复杂度为$O(N)$，其中$T$为会议区间相交集数目。

执行用时：85ms，在所有java提交中击败了36.41%的用户。

内存消耗：82.6MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。