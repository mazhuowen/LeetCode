[toc]

Write a class `RecentCounter` to count recent requests.

It has only one method: `ping(int t)`, where t represents some time in milliseconds.

Return the number of `pings` that have been made from 3000 milliseconds ago until now.

Any ping with time in `[t - 3000, t]` will count, including the current ping.

It is guaranteed that every call to `ping` uses a strictly larger value of t than before.



Note:

* Each test case will have at most 10000 calls to ping.
* Each test case will call ping with strictly increasing values of t.
* Each call to ping will have $1 \le t \le 10^9$.



## 题目解读

&emsp;题目要求防范返回3000毫秒前到现在的所有请求数。

```java
class RecentCounter {

    public RecentCounter() {
        
    }
    
    public int ping(int t) {
        
    }
}

/**
 * Your RecentCounter object will be instantiated and called as such:
 * RecentCounter obj = new RecentCounter();
 * int param_1 = obj.ping(t);
 */
```

## 程序设计

* 使用队列记录请求，每次方法调用入队当前请求，出队3000毫秒前的请求，返回队列元素数。

```java
class RecentCounter {
    private Queue<Integer> queue;

    public RecentCounter() {
        queue = new LinkedList<>();
    }
    
    public int ping(int t) {
        // 出队不满足条件的请求
        while(!queue.isEmpty() && queue.peek() < t - 3000) {
            queue.poll();
        }
        // 入队当前请求
        queue.add(t);
        return queue.size();
    }
}
```

## 性能分析

&emsp;时间复杂度最坏情况$O(N)$，空间复杂度$O(N)$。

执行用时：42ms，在所有java提交中击败了61.26%的用户。

内存消耗：64.4MB，在所有java提交中击败了73.35%的用户。

## 官方解题

&emsp;思路一致，可以将入队放在前面，免去出队判断队空的判断。

```java
class RecentCounter {
    private Queue<Integer> queue;

    public RecentCounter() {
        queue = new LinkedList<>();
    }
    
    public int ping(int t) {
        queue.add(t);
        while(queue.peek() < t - 3000) {
            queue.poll();
        }
        return queue.size();
    }
}
```

执行用时：39ms，在所有java提交中击败了92.12%的用户。

内存消耗：65.4MB，在所有java提交中击败了47.36%的用户。