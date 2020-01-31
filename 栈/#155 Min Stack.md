[toc]

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

* push(x) -- Push element x onto stack.
* pop() -- Removes the element on top of the stack.
* top() -- Get the top element.
* getMin() -- Retrieve the minimum element in the stack.



## 题目解读

&emsp;设计一个栈结构，除了常规功能，需要在常数时间内检索到最小元素。

```java
class MinStack {

    /** initialize your data structure here. */
    public MinStack() {
        
    }
    
    public void push(int x) {
        
    }
    
    public void pop() {
        
    }
    
    public int top() {
        
    }
    
    public int getMin() {
        
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```

## 程序设计

* 首先栈可以采用数组或链表两种形式；考虑到还要维护最小值信息，可以采用两个链表，一个维护栈信息，一个维护排序信息，但这样会使数据结构复杂；可以使用单链表实现栈，每个结点除了保存当前值，还要保存当前结点对应的栈的最小值。

```java
class MinStack {
    private Node stack;

    private class Node{
        // 结点值
        private int val;
        // 当前结点对应栈的最小值
        private int min;
        // 前一个元素
        private Node next;

        Node(int val, int min, Node next){
            this.val = val;
            this.min = min;
            this.next = next;
        }
    }

    /** initialize your data structure here. */
    public MinStack() {
        stack = null;
    }
    
    public void push(int x) {
        if(stack == null) {
            stack = new Node(x, x, null);
        } else {
            stack = new Node(x, Math.min(x, stack.min), stack);
        }
    }
    
    public void pop() {
        stack = stack.next;
    }
    
    public int top() {
        return stack.val;
    }
    
    public int getMin() {
        return stack.min;
    }
}
```

## 性能分析

&emsp;所有方法时间复杂度都是$O(1)$。

执行用时：6ms，在所有java提交中击败了97.77%的用户。

内存消耗：40.2MB，在所有java提交中击败了69.00%的用户。

## 官方解题

&emsp;暂无，密切关注。