[toc]

Design a max stack that supports push, pop, top, peekMax and popMax.

* `push(x)` -- Push element x onto stack.
* `pop()` -- Remove the element on top of the stack and return it.
* `top()` -- Get the element on the top.
* `peekMax()` -- Retrieve the maximum element in the stack.
* `popMax()` -- Retrieve the maximum element in the stack, and remove it. If you find more than one maximum elements, only remove the top-most one.



Note:

* $-1e7 \le x \le 1e7$
* Number of operations won't exceed $10000$.
* The last four operations won't be called when stack is empty.



## 题目解读

&emsp;设计最大栈。

```java
class MaxStack {

    /** initialize your data structure here. */
    public MaxStack() {

    }
    
    public void push(int x) {

    }
    
    public int pop() {

    }
    
    public int top() {

    }
    
    public int peekMax() {

    }
    
    public int popMax() {

    }
}

/**
 * Your MaxStack object will be instantiated and called as such:
 * MaxStack obj = new MaxStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.peekMax();
 * int param_5 = obj.popMax();
 */
```

## 程序设计

* 采用双栈操作。

```java
class MaxStack {
    // 数据栈
    Stack<Integer> dataStack;
    // 保存每个数据在栈时栈中最大值
    Stack<Integer> maxStack;

    /** initialize your data structure here. */
    public MaxStack() {
        this.dataStack = new Stack<>();
        this.maxStack = new Stack<>();
    }
    
    public void push(int x) {
        dataStack.push(x);
        // 为主当前最大值
        if (maxStack.isEmpty() || x >= maxStack.peek()) maxStack.push(x);
        else maxStack.push(maxStack.peek());
    }
    
    public int pop() {
        if (dataStack.isEmpty()) throw new RuntimeException("stack is empty");
        maxStack.pop();
        return dataStack.pop();
    }
    
    public int top() {
        if (dataStack.isEmpty()) throw new RuntimeException("stack is empty");
        return dataStack.peek();
    }
    
    public int peekMax() {
        if (dataStack.isEmpty()) throw new RuntimeException("stack is empty");
        return maxStack.peek();
    }
    
    public int popMax() {
        if (dataStack.isEmpty()) throw new RuntimeException("stack is empty");
        // 遍历最大值值，将最大值前面的数值放入临时栈
        Stack<Integer> temp = new Stack<>();
        int max = maxStack.peek();
        while (dataStack.peek() != max) {
            temp.push(dataStack.pop());
            maxStack.pop();
        }
        dataStack.pop();
        maxStack.pop();
        // 将临时栈数据重新加入主栈
        while (!temp.isEmpty()) {
            push(temp.pop());
        }
        return max;
    }
}
```

## 性能分析

&emsp;除了移除最大值操作为$O(N)$，其它操作时间复杂度为$O(1)$，空间复杂度为$O(N)$。

执行用时：24ms，在所有java提交中击败了78.93%的用户。

内存消耗：42.2MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方使用红黑树记录大小，使用链表记录插入顺序，组合实现最大栈。