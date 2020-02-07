[toc]

Design your implementation of the circular queue. The circular queue is a linear data structure in which the operations are performed based on FIFO (First In First Out) principle and the last position is connected back to the first position to make a circle. It is also called "Ring Buffer".

One of the benefits of the circular queue is that we can make use of the spaces in front of the queue. In a normal queue, once the queue becomes full, we cannot insert the next element even if there is a space in front of the queue. But using the circular queue, we can use the space to store new values.

Your implementation should support following operations:

* `MyCircularQueue(k)`: Constructor, set the size of the queue to be k.
* `Front`: Get the front item from the queue. If the queue is empty, return -1.
* `Rear`: Get the last item from the queue. If the queue is empty, return -1.
* `enQueue(value)`: Insert an element into the circular queue. Return true if the operation is successful.
* `deQueue()`: Delete an element from the circular queue. Return true if the operation is successful.
* `isEmpty()`: Checks whether the circular queue is empty or not.
* `isFull()`: Checks whether the circular queue is full or not.

Note:

* All values will be in the range of [0, 1000].
* The number of operations will be in the range of [1, 1000].
* Please do not use the built-in Queue library.



## 题目解读

&emsp;设计循环队列。

```java
class MyCircularQueue {

    /** Initialize your data structure here. Set the size of the queue to be k. */
    public MyCircularQueue(int k) {
        
    }
    
    /** Insert an element into the circular queue. Return true if the operation is successful. */
    public boolean enQueue(int value) {
        
    }
    
    /** Delete an element from the circular queue. Return true if the operation is successful. */
    public boolean deQueue() {
        
    }
    
    /** Get the front item from the queue. */
    public int Front() {
        
    }
    
    /** Get the last item from the queue. */
    public int Rear() {
        
    }
    
    /** Checks whether the circular queue is empty or not. */
    public boolean isEmpty() {
        
    }
    
    /** Checks whether the circular queue is full or not. */
    public boolean isFull() {
        
    }
}

/**
 * Your MyCircularQueue object will be instantiated and called as such:
 * MyCircularQueue obj = new MyCircularQueue(k);
 * boolean param_1 = obj.enQueue(value);
 * boolean param_2 = obj.deQueue();
 * int param_3 = obj.Front();
 * int param_4 = obj.Rear();
 * boolean param_5 = obj.isEmpty();
 * boolean param_6 = obj.isFull();
 */
```

## 程序设计

* 循环队列的设计，`front`作为标识位，指向队首，`rear`指向队尾；队空则`front =rear`，队列满则`front%size = (rear + 1)%size`；队首元素索引位置为`(front + 1)%size`，队尾索引位置为`rear%size`。

```java
class MyCircularQueue {
    private int front;
    private int rear;
    private int[] queue;

    /** Initialize your data structure here. Set the size of the queue to be k. */
    public MyCircularQueue(int k) {
        this.queue = new int[k + 1];
        this.front = 0;
        this.rear = 0;
    }
    
    /** Insert an element into the circular queue. Return true if the operation is successful. */
    public boolean enQueue(int value) {
        // 判断队列是否已满
        if(front % queue.length == (rear + 1) % queue.length) {
            return false;
        }
        // 入队，队尾加一
        queue[(++rear) % queue.length] = value;
        return true;
    }
    
    /** Delete an element from the circular queue. Return true if the operation is successful. */
    public boolean deQueue() {
        // 判断队列是否为空
        if(front == rear) {
            return false;
        }
        // 队首加一
        front++;
        return true;
    }
    
    /** Get the front item from the queue. */
    public int Front() {
        // 判断队空
        if(front == rear) {
            return -1;
        }
        // 返回队首
        return queue[(front + 1) % queue.length];
    }
    
    /** Get the last item from the queue. */
    public int Rear() {
        // 判断队空
        if(rear == front) {
            return -1;
        }
        // 返回队尾
        return queue[rear % queue.length];
    }
    
    /** Checks whether the circular queue is empty or not. */
    public boolean isEmpty() {
        // 判断队空
        return front == rear;
    }
    
    /** Checks whether the circular queue is full or not. */
    public boolean isFull() {
        // 判断队列是否已满
        return front % queue.length == (rear + 1) % queue.length;
    }
}
```

## 性能分析

&emsp;各个操作时间复杂度为$O(1)$，空间复杂度为$O(N)$。

执行用时：6ms，在所有java提交中击败了98.88%的用户。

内存消耗：37.4MB，在所有java提交中击败了98.88%的用户。

## 官方解题

&emsp;暂无，密切关注。