[toc]

Design a stack which supports the following operations.

Implement the `CustomStack` class:

* `CustomStack(int maxSize)` Initializes the object with `maxSize` which is the maximum number of elements in the stack or do nothing if the stack reached the `maxSize`.
* `void push(int x)` Adds `x` to the top of the stack if the stack hasn't reached the `maxSize`.
* `int pop()` Pops and returns the top of stack or -1 if the stack is empty.
* `void inc(int k, int val)` Increments the bottom `k` elements of the stack by `val`. If there are less than `k` elements in the stack, just increment all the elements in the stack.



Constraints:

* $1 \le \text{maxSize} \le 1000$
* $1 \le x \le 1000$
* $1 \le k \le 1000$
* $0 \le \text{val} \le 100$
* At most `1000` calls will be made to each method of `increment`, `push` and `pop` each separately.



## 题目解读

&emsp;设计栈，可批量增加栈底元素值。

```java
class CustomStack {

    public CustomStack(int maxSize) {

    }
    
    public void push(int x) {

    }
    
    public int pop() {

    }
    
    public void increment(int k, int val) {

    }
}

/**
 * Your CustomStack object will be instantiated and called as such:
 * CustomStack obj = new CustomStack(maxSize);
 * obj.push(x);
 * int param_2 = obj.pop();
 * obj.increment(k,val);
 */
```

## 程序设计

* 其他操作都是基础的栈操作，重点讨论批量增加。最基本的思路是每次增加都遍历相应的数值并维护新的数值，这样时间复杂度为$O(N^2)$。考虑到栈的特性，先进后出，每个元素出栈都是依照顺序，只有栈顶元素才可以出栈，这样可以引入增加值数组，记录增加值的操作，底层$k$个增加$val$，则在数组$k - 1$处加入$val$，当元素出栈时检查增加数组，如果有值$val$则添加到栈顶，同时将$val$添加到增加值数组的下一个位置，即懒操作。

```java
class CustomStack {
    int maxSize;
    int size;

    // 栈
    int[] stack;
    // 记录增量
    int[] incre;

    public CustomStack(int maxSize) {
        this.maxSize = maxSize;
        this.stack = new int[maxSize];
        this.incre = new int[maxSize];
    }
    
    public void push(int x) {
        if (size == maxSize) return;
        stack[size++] = x;
    }
    
    public int pop() {
        if (size == 0) return -1;
        size--;
        int val = stack[size];
        int inc = incre[size];
        // 重置，避免叠加
        incre[size] = 0;
        if (inc != 0 && size > 0) incre[size - 1] += inc;
        return val + inc;
    }
    
    public void increment(int k, int val) {
        if (k <= 0 || size <= 0) return;
        if (k >= size) incre[size - 1] += val;
        else incre[k - 1] += val;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(N)$。

执行用时：6ms，在所有java提交中击败了99.71%的用户。

内存消耗：40.8MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。