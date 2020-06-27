[toc]

Implement `FreqStack`, a class which simulates the operation of a stack-like data structure.

`FreqStack` has two functions:

* `push(int x)`, which pushes an integer x onto the stack.
* `pop()`, which **removes** and returns the most frequent element in the stack.
  * If there is a tie for most frequent element, the element closest to the top of the stack is removed and returned.



**Note**:

* Calls to `FreqStack.push(int x)` will be such that $0 \le x \le 10^9$.
* It is guaranteed that `FreqStack.pop()` won't be called if the stack has zero elements.
* The total number of `FreqStack.push` calls will not exceed `10000` in a single test case.
* The total number of `FreqStack.pop` calls will not exceed `10000` in a single test case.
* The total number of `FreqStack.push` and `FreqStack.pop` calls will not exceed `150000` across all test cases.



## 题目解读

&emsp;设计数据结构，使得出栈频数最多的元素。

```java
class FreqStack {

    public FreqStack() {

    }
    
    public void push(int x) {

    }
    
    public int pop() {

    }
}

/**
 * Your FreqStack object will be instantiated and called as such:
 * FreqStack obj = new FreqStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 */
```

## 程序设计

* 由于每个元素频率的变化都是连续的，即在原来的基础上加减一，最开始想到的是维护链表结构，每个节点表示一个频率，每个节点后紧跟该频率的数字；这样最大频率链表的末尾就是出栈值。
* 参考官方思路，官方使用字典和栈组合更轻松优雅实现上述逻辑。

```java
class FreqStack {
    Map<Integer, Integer> freq;
    Map<Integer, Stack<Integer>> group;
    int maxFreq;

    public FreqStack() {
        this.freq = new HashMap<>();
        this.group = new HashMap<>();
    }
    
    public void push(int x) {
        int f = freq.getOrDefault(x, 0) + 1;
        freq.put(x, f);
        maxFreq = Math.max(maxFreq, f);
        if (group.get(f) == null) group.put(f, new Stack<>());
        group.get(f).push(x);
    }
    
    public int pop() {
        int val = group.get(maxFreq).pop();
        freq.put(val, freq.get(val) - 1);
        if (group.get(maxFreq).isEmpty()) maxFreq--;
        return val;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(N)$。

执行用时：39ms，在所有java提交中击败了86.08%的用户。

内存消耗：48.7MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;上述思路参考官方解题。