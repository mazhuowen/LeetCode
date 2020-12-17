[toc]

请定义一个队列并实现函数 `max_value` 得到队列里的最大值，要求函数`max_value`、`push_back` 和 `pop_front` 的均摊时间复杂度都是$O(1)$。

若队列为空，`pop_front` 和 `max_value` 需要返回 $-1$



**示例 1**：

```
输入: 
["MaxQueue","push_back","push_back","max_value","pop_front","max_value"]
[[],[1],[2],[],[],[]]
输出: [null,null,null,2,1,2]
```

**示例 2**：

```
输入: 
["MaxQueue","pop_front","max_value"]
[[],[],[]]
输出: [null,-1,-1]
```



**限制**：

* $1 \le \text{push_back,pop_front,max_value的总操作数} \le 10000$
* $1 \le \text{value} \le 10^5$



## 题目解读

&emsp;设计队列，使得队列可返回最大值。

```java
class MaxQueue {

    public MaxQueue() {

    }
    
    public int max_value() {

    }
    
    public void push_back(int value) {

    }
    
    public int pop_front() {

    }
}

/**
 * Your MaxQueue object will be instantiated and called as such:
 * MaxQueue obj = new MaxQueue();
 * int param_1 = obj.max_value();
 * obj.push_back(value);
 * int param_3 = obj.pop_front();
 */
```

## 程序设计

* 由于题目要求均摊时间复杂度为$O(1)$，可维护最大值队列，若当前入队值大于之前的所有值，则可出队这些值，如果小于等于之前的值，可入队。

```java
class MaxQueue {
    // 队列
    Queue<Integer> queue;
    // 递减队列
    Deque<Integer> desSeq;

    public MaxQueue() {
        this.queue = new LinkedList<>();
        this.desSeq = new LinkedList<>();
    }
    
    public int max_value() {
        if (queue.isEmpty()) return -1;
        return desSeq.peekFirst();
    }
    
    public void push_back(int value) {
        // 维护递减队列，将小于的值出队
        while (!desSeq.isEmpty() && value > desSeq.peekLast()) desSeq.pollLast();
        desSeq.add(value);
        queue.add(value);
    }
    
    public int pop_front() {
        if (queue.isEmpty()) return -1;
        int val = queue.poll();
        // 递减队列同步出队
        if (!desSeq.isEmpty() && desSeq.peekFirst() == val) desSeq.pollFirst();
        return val;
    }
}
```

## 性能分析

&emsp;出队、返回最大值时间复杂度为$O(1)$，对于入队，由于需要出队递减队列，时间复杂度必然大于$O(1)$，但总体来看，对于$N$次操作，出队数目不超过$N$，故平摊时间复杂度为$O(1)$。

执行用时：41 ms, 在所有 Java 提交中击败了36.80%的用户。

内存消耗：46.6 MB, 在所有 Java 提交中击败了54.77%的用户。

## 官方解题

&emsp;上述思路参考官方解题。
