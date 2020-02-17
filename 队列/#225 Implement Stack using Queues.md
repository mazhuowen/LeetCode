[toc]

Implement the following operations of a stack using queues.

* `push(x)` -- Push element x onto stack.
* `pop()` -- Removes the element on top of the stack.
* `top()` -- Get the top element.
* `empty()` -- Return whether the stack is empty.

Notes:

* You must use only standard operations of a queue -- which means only push to back, peek/pop from front, size, and is empty operations are valid.
* Depending on your language, queue may not be supported natively. You may simulate a queue by using a list or deque (double-ended queue), as long as you use only standard operations of a queue.
* You may assume that all operations are valid (for example, no pop or top operations will be called on an empty stack).



## 题目解读

&emsp;设计使用队列实现栈的功能。

```java
class MyStack {

    /** Initialize your data structure here. */
    public MyStack() {
        
    }
    
    /** Push element x onto stack. */
    public void push(int x) {
        
    }
    
    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        
    }
    
    /** Get the top element. */
    public int top() {
        
    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        
    }
}

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * boolean param_4 = obj.empty();
 */
```

## 程序设计

* 双向队列可以实现栈的操作。

```java
class MyStack {
    private Deque<Integer> queue;

    /** Initialize your data structure here. */
    public MyStack() {
        queue = new LinkedList<>();
    }
    
    /** Push element x onto stack. */
    public void push(int x) {
        queue.addLast(x);
    }
    
    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        return queue.removeLast();
    }
    
    /** Get the top element. */
    public int top() {
        return queue.getLast();
    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        return queue.isEmpty();
    }
}

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * boolean param_4 = obj.empty();
 */
```

## 性能分析

&emsp;所有操作时间复杂度为$O(1)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：40.5MB，在所有java提交中击败了5.41%的用户。

## 官方解题

&emsp;基础解法，手动实现，不再重述。