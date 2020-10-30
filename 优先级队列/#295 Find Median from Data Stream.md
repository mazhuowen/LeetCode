[toc]

Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.

Design a data structure that supports the following two operations:

* `void addNum(int num)` - Add a integer number from the data stream to the data structure.
* `double findMedian()` - Return the median of all elements so far.



**Follow up**:

* If all integer numbers from the stream are between 0 and 100, how would you optimize it?
* If 99% of all integer numbers from the stream are between 0 and 100, how would you optimize it?



## 题目解读

&emsp;设计数据结构，计算数据流的中位数。

```java
class MedianFinder {

    /** initialize your data structure here. */
    public MedianFinder() {
        
    }
    
    public void addNum(int num) {
        
    }
    
    public double findMedian() {
        
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```

## 程序设计

* 首先题目中数据流不是顺序的，所以需要动态排序；其次数据不能丢弃，防止后续插入小的值，导致中位数不能回溯。
* 可以使用一个堆来记录排序数据，一个整数来记录总的数据数，当查中位数时出队直到得到中位数，需要注意，出队的数据需要保存并重新入队。
* 进一步考虑可以使用两个堆，一个最大堆来记录中位数前面的数据，一个最小堆来记录中位数及后面的数据。显然最大堆的堆顶数据不能大于最小堆的堆顶数据。每次加入数据，就是维护两个堆的关系。而获取中位数只是简单的判断取值。

```java
class MedianFinder {
    // 前半部分最大堆
    private PriorityQueue<Integer> pre;
    // 后半部分最小堆
    private PriorityQueue<Integer> next;

    /** initialize your data structure here. */
    public MedianFinder() {
        pre = new PriorityQueue<>((a, b) -> b - a);
        next = new PriorityQueue<>();
    }
    
    public void addNum(int num) {
        next.add(num);
        // 两个堆尺寸只能相差1以内
        if(next.size() - pre.size() > 1) {
            pre.add(next.poll());
        }
        // 重复直到next堆大于pre堆
        while(!pre.isEmpty() && !next.isEmpty() && pre.peek() > next.peek()) {
            int temp = pre.poll();
            pre.add(next.poll());
            next.add(temp);
        }
    }
    
    public double findMedian() {
        // 偶数返回均值
        if(next.size() == pre.size()) {
            return (pre.peek() + next.peek()) / 2.0;
        } 
        // 奇数返回堆顶
        else {
            return next.peek();
        }
    }
}
```

## 性能分析

&emsp;最好情况数据是按增序加入，此时加入数据的时间复杂度为$O(\log_2N)$；最坏情况按降序加入，此时时间复杂度也为$O(\log_2N)$；获取中位数的时间复杂度为$O(1)$。空间复杂度为$O(N)$。

执行用时：99ms，在所有java提交中击败了57.85%的用户。

内存消耗：78.9MB，在所有java提交中击败了5.14%的用户。

## 官方解题

&emsp;官方解题除了双堆，还提供了最基础的每次将数据加入到链表，然后取中位数时排序并返回中位数。