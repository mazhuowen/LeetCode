[toc]

Design your implementation of the circular double-ended queue (deque).

Your implementation should support following operations:

* `MyCircularDeque(k)`: Constructor, set the size of the deque to be k.
* `insertFront()`: Adds an item at the front of Deque. Return true if the operation is successful.
* `insertLast()`: Adds an item at the rear of Deque. Return true if the operation is successful.
* `deleteFront()`: Deletes an item from the front of Deque. Return true if the operation is successful.
* `deleteLast()`: Deletes an item from the rear of Deque. Return true if the operation is successful.
* `getFront()`: Gets the front item from the Deque. If the deque is empty, return -1.
* `getRear()`: Gets the last item from Deque. If the deque is empty, return -1.
* `isEmpty()`: Checks whether Deque is empty or not. 
* `isFull()`: Checks whether Deque is full or not.



Note:

* All values will be in the range of [0, 1000].
* The number of operations will be in the range of [1, 1000].
* Please do not use the built-in Deque library.



## 题目解读

&emsp;设计双向循环队列。

```java
class MyCircularDeque {

    /** Initialize your data structure here. Set the size of the deque to be k. */
    public MyCircularDeque(int k) {
        
    }
    
    /** Adds an item at the front of Deque. Return true if the operation is successful. */
    public boolean insertFront(int value) {
        
    }
    
    /** Adds an item at the rear of Deque. Return true if the operation is successful. */
    public boolean insertLast(int value) {
        
    }
    
    /** Deletes an item from the front of Deque. Return true if the operation is successful. */
    public boolean deleteFront() {
        
    }
    
    /** Deletes an item from the rear of Deque. Return true if the operation is successful. */
    public boolean deleteLast() {
        
    }
    
    /** Get the front item from the deque. */
    public int getFront() {
        
    }
    
    /** Get the last item from the deque. */
    public int getRear() {
        
    }
    
    /** Checks whether the circular deque is empty or not. */
    public boolean isEmpty() {
        
    }
    
    /** Checks whether the circular deque is full or not. */
    public boolean isFull() {
        
    }
}

/**
 * Your MyCircularDeque object will be instantiated and called as such:
 * MyCircularDeque obj = new MyCircularDeque(k);
 * boolean param_1 = obj.insertFront(value);
 * boolean param_2 = obj.insertLast(value);
 * boolean param_3 = obj.deleteFront();
 * boolean param_4 = obj.deleteLast();
 * int param_5 = obj.getFront();
 * int param_6 = obj.getRear();
 * boolean param_7 = obj.isEmpty();
 * boolean param_8 = obj.isFull();
 */
```

## 程序设计

* 双向循环队列比单向循环队列对了队首的插入和队尾的删除；多的操作使得判断队空由`front==rear`变为`front%size==rear%size`。插入队首需要判断队首位置是否是0；删除队尾需要判断队尾是否是0。

```java
class MyCircularDeque {
    private int front;
    private int rear;
    private int[] queue;

    /** Initialize your data structure here. Set the size of the deque to be k. */
    public MyCircularDeque(int k) {
        this.queue = new int[k + 1];
        this.front = 0;
        this.rear = 0;
    }
    
    /** Adds an item at the front of Deque. Return true if the operation is successful. */
    public boolean insertFront(int value) {
        if(isFull()) {
            return false;
        }
        // front为0，则front后移到size-1的位置，将队首元素放入位置0
        if(front == 0) {
            queue[0] = value;
            front = queue.length - 1;
        } 
        // 执行front--
        else {
            queue[(front--) % queue.length] = value;
        }
        return true;
    }
    
    /** Adds an item at the rear of Deque. Return true if the operation is successful. */
    public boolean insertLast(int value) {
        // 判断队列已满
        if(isFull()) {
            return false;
        }
        queue[(++rear) % queue.length] = value;
        return true;
    }
    
    /** Deletes an item from the front of Deque. Return true if the operation is successful. */
    public boolean deleteFront() {
        // 判断队列是否空
        if(isEmpty()) {
            return false;
        }
        front++;
        return true;
    }
    
    /** Deletes an item from the rear of Deque. Return true if the operation is successful. */
    public boolean deleteLast() {
        if(isEmpty()) {
            return false;
        }
        // 队尾为0，则队尾前移一位
        if(rear == 0) {
            rear = queue.length - 1;
        } 
        // 队尾前移
        else{
            rear--;
        }
        return true;
    }
    
    /** Get the front item from the deque. */
    public int getFront() {
        if(isEmpty()) {
            return -1;
        }
        return queue[(front + 1) % queue.length];
    }
    
    /** Get the last item from the deque. */
    public int getRear() {
        if(isEmpty()) {
            return -1;
        }
        return queue[rear % queue.length];
    }
    
    /** Checks whether the circular deque is empty or not. */
    public boolean isEmpty() {
        // 判空条件需要取余
        return front % queue.length == rear % queue.length;
    }
    
    /** Checks whether the circular deque is full or not. */
    public boolean isFull() {
        return front % queue.length == (rear + 1) % queue.length;
    }
}
```

## 性能分析

&emsp;各操作时间复杂度$O(1)$，空间复杂度$O(N)$。

执行用时：6ms，在所有java提交中击败了99.70%的用户。

内存消耗：38.8MB，在所有java提交中击败了13.49%的用户。

## 官方解题

&emsp;暂无，密切关注。