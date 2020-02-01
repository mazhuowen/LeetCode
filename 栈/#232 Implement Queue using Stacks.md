[toc]

Implement the following operations of a queue using stacks.

* push(x) -- Push element x to the back of queue.
* pop() -- Removes the element from in front of queue.
* peek() -- Get the front element.
* empty() -- Return whether the queue is empty.



Notes:

* You must use only standard operations of a stack -- which means only `push to top`, `peek/pop from top`, `size`, and `is empty` operations are valid.
* Depending on your language, stack may not be supported natively. You may simulate a stack by using a list or deque (double-ended queue), as long as you use only standard operations of a stack.
* You may assume that all operations are valid (for example, no pop or peek operations will be called on an empty queue).



## 题目解读

&emsp;使用栈来实现队列的功能。题目限定只能使用基本的栈操作。题目保证操作都是有效的，及出队列的次数不会大于入队的数目。

```java
class MyQueue {

    /** Initialize your data structure here. */
    public MyQueue() {
        
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        
    }
    
    /** Get the front element. */
    public int peek() {
        
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        
    }
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.empty();
 */
```

## 程序设计

* 栈先进后出，队列先进先出，如果要用栈实现队列，需要两个栈，一个栈用于入队操作，另一个栈用于出对操作，两个栈需要来回转换。

```java
class MyQueue {
    private Stack<Integer> stack;
    private Stack<Integer> temp;

    /** Initialize your data structure here. */
    public MyQueue() {
        // 用于入队
        stack = new Stack<>();
        // 用于出队
        temp = new Stack<>();
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        // 如果temp不为空，将temp“倒入”stack，完成入队
        while(!temp.isEmpty()) {
            stack.push(temp.pop());
        }
        stack.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        // 出队操作，satck倒入temp
        while(!stack.isEmpty()) {
            temp.push(stack.pop());
        }
        return temp.pop();
    }
    
    /** Get the front element. */
    public int peek() {
        // satck倒入temp
        while(!stack.isEmpty()) {
            temp.push(stack.pop());
        }
        return temp.peek();
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        // 判空，需要两个同时判断
        return stack.isEmpty() && temp.isEmpty();
    }
}
```

## 性能分析

&emsp;最好的情况下，连续入队或连续出队，时间复杂度$O(1)$，最坏的情况，入队出队交替进行，时间复杂度$O(N)$。空间复杂度$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：34MB，在所有java提交中击败了39.22%的用户。

## 官方解题

&emsp;与上面类似。