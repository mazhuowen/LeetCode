[toc]

Implement a thread safe bounded blocking queue that has the following methods:

* `BoundedBlockingQueue(int capacity)` The constructor initializes the queue with a maximum `capacity`.
* `void enqueue(int element)` Adds an `element` to the front of the queue. If the queue is full, the calling thread is blocked until the queue is no longer full.
* `int dequeue()` Returns the element at the rear of the queue and removes it. If the queue is empty, the calling thread is blocked until the queue is no longer empty.
* `int size()` Returns the number of elements currently in the queue.

Your implementation will be tested using multiple threads at the same time. Each thread will either be a producer thread that only makes calls to the `enqueue` method or a consumer thread that only makes calls to the `dequeue` method. The `size` method will be called after every test case.

Please do not use built-in implementations of bounded blocking queue as this will not be accepted in an interview.



## 题目解读

&emsp;设计阻塞队列。

```java
class BoundedBlockingQueue {

    public BoundedBlockingQueue(int capacity) {
        
    }
    
    public void enqueue(int element) throws InterruptedException {
        
    }
    
    public int dequeue() throws InterruptedException {
        
    }
    
    public int size() {
        
    }
}
```

## 程序设计

* 使用条件锁来实现入队出队间通信。

```java
class BoundedBlockingQueue {
    Lock lock;
    // 入队出队条件锁
    Condition enq;
    Condition deq;

    int capacity;
    int size;
    Queue<Integer> queue;

    public BoundedBlockingQueue(int capacity) {
        this.lock = new ReentrantLock();
        this.enq = lock.newCondition();
        this.deq = lock.newCondition();
        this.capacity = capacity;
        this.queue = new LinkedList<>();
    }
    
    public void enqueue(int element) throws InterruptedException {
        lock.lock();
        try {
            // 若队列已满，则等待空位
            while (size == capacity) enq.await();
            queue.add(element);
            size++;
            deq.signalAll();
        } finally {
            lock.unlock();
        }
    }
    
    public int dequeue() throws InterruptedException {
        lock.lock();
        try {
            // 若队列为空，等待入队
            while (size == 0) deq.await();
            size--;
            int res = queue.poll();
            enq.signalAll();
            return res;
        } finally {
            lock.unlock();
        }
    }
    
    public int size() {
        lock.lock();
        try {
            return size;
        } finally {
            lock.unlock();
        }
    }
}
```

## 性能分析

执行用时：10ms，在所有java提交中击败了96.10%的用户。

内存消耗：39.6MB，在所有java提交中击败了33.33%的用户。

## 官方解题

&emsp;暂无，密切关注。