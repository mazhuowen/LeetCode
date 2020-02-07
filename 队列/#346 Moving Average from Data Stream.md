[toc]

Given a stream of integers and a window size, calculate the moving average of all integers in the sliding window.



## 题目解读

&emsp;给定数据流，计算窗口内数据的均值，实质就是计算队列元素的均值，队列有尺寸限制，当达到窗口大小时需要先出队，再进队。

```java
class MovingAverage {

    /** Initialize your data structure here. */
    public MovingAverage(int size) {
        
    }
    
    public double next(int val) {
        
    }
}

/**
 * Your MovingAverage object will be instantiated and called as such:
 * MovingAverage obj = new MovingAverage(size);
 * double param_1 = obj.next(val);
 */
```

## 程序设计

* 需要队列来维护窗口数据；总和来记录窗口元素值之和，避免遍历；窗口尺寸来判断队列的出队、入队。

```java
class MovingAverage {
    private double sum = 0.0;
    private int size;
    private Queue<Integer> queue;

    /** Initialize your data structure here. */
    public MovingAverage(int size) {
        this.size = size;
        this.queue = new LinkedList<>();
    }
    
    public double next(int val) {
        // 达到要求，出队
        if(queue.size() == size) {
            sum -= queue.poll();
        }
        // 入队
        queue.add(val);
        sum += val;
        return sum / queue.size();
    }
}
```

* 由于是固定窗口尺寸，可以想到顺序实现的循环队列，且在本题中不需要判断队列是否已满，只需要覆盖入队即可。

```java
class MovingAverage {
    private double sum = 0.0;
    private int tail;
    private int[] queue;

    /** Initialize your data structure here. */
    public MovingAverage(int size) {
        queue = new int[size];
        tail = 0;
    }
    
    public double next(int val) {
        // 减去需要出队的值
        if(tail >= queue.length) {
            sum -= queue[tail % queue.length];
        }
        // 入队覆盖
        queue[tail % queue.length] = val;
        sum += val;
        // 尾结点移到下一次入队的位置
        tail++;
        // 判断窗口值，根据尾结点值判断
        if(tail < queue.length) {
            // 窗口未满，除元素个数（上面tail++，此时正好是元素数）
            return sum / tail;
        } else {
            // 窗口已满，除窗口值
            return sum / queue.length;
        }
    }
}
```

## 性能分析

&emsp;两种方法时间复杂度为$O(1)$，空间复杂度为$O(N)$。方法一：

执行用时：33ms，在所有java提交中击败了78.81%的用户。

内存消耗：42.6MB，在所有java提交中击败了22.99%的用户。

方法二：

执行用时：31ms，在所有java提交中击败了98.45%的用户。

内存消耗：42MB，在所有java提交中击败了69.73%的用户。

## 官方解题

&emsp;思路同上。