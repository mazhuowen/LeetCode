[toc]

Design a hit counter which counts the number of hits received in the past 5 minutes.

Each function accepts a timestamp parameter (in seconds granularity) and you may assume that calls are being made to the system in chronological order (ie, the timestamp is monotonically increasing). You may assume that the earliest timestamp starts at 1.

It is possible that several hits arrive roughly at the same time.



Follow up:
What if the number of hits per second could be very large? Does your design scale?



## 题目解读

&emsp;设计计数器。

```java
class HitCounter {

    /** Initialize your data structure here. */
    public HitCounter() {

    }
    
    /** Record a hit.
        @param timestamp - The current timestamp (in seconds granularity). */
    public void hit(int timestamp) {

    }
    
    /** Return the number of hits in the past 5 minutes.
        @param timestamp - The current timestamp (in seconds granularity). */
    public int getHits(int timestamp) {

    }
}

/**
 * Your HitCounter object will be instantiated and called as such:
 * HitCounter obj = new HitCounter();
 * obj.hit(timestamp);
 * int param_2 = obj.getHits(timestamp);
 */
```

## 程序设计

* 最基本的，使用队列实现，每个操作都与队首元素比较，注意`getHits`不入队。

```java
class HitCounter {
    private Queue<Integer> queue;

    /** Initialize your data structure here. */
    public HitCounter() {
        this.queue = new LinkedList<>();
    }
    
    /** Record a hit.
        @param timestamp - The current timestamp (in seconds granularity). */
    public void hit(int timestamp) {
        // 将超过5分钟的时间取出
        while (!queue.isEmpty() && queue.peek() + 300 <= timestamp) {
            queue.poll();
        }
        queue.add(timestamp);
    }
    
    /** Return the number of hits in the past 5 minutes.
        @param timestamp - The current timestamp (in seconds granularity). */
    public int getHits(int timestamp) {
        // 将超过5分钟的时间取出
        while (!queue.isEmpty() && queue.peek() + 300 <= timestamp) {
            queue.poll();
        }
        return queue.size();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.1MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;暂无，密切关注。