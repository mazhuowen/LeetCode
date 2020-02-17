[toc]

Design a class to find the **k**th largest element in a stream. Note that it is the kth largest element in the sorted order, not the kth distinct element.

Your `KthLargest` class will have a constructor which accepts an integer k and an integer array `nums`, which contains initial elements from the stream. For each call to the method `KthLargest.add`, return the element representing the kth largest element in the stream.

**Note:**
You may assume that $\text{nums' length} \ge k-1$ and $k \ge 1$.



## 题目解读

&emsp;设计类，返回数据流中的第$k$大数据。

```java
class KthLargest {

    public KthLargest(int k, int[] nums) {
        
    }
    
    public int add(int val) {
        
    }
}

/**
 * Your KthLargest object will be instantiated and called as such:
 * KthLargest obj = new KthLargest(k, nums);
 * int param_1 = obj.add(val);
 */
```

## 程序设计

* 使用最小堆来实现top-k问题。

```java
class KthLargest {
    private int k;
    private PriorityQueue<Integer> queue;

    public KthLargest(int k, int[] nums) {
        this.k = k;
        // 最小堆
        this.queue = new PriorityQueue<>(k);
        for(int num : nums) {
            if(this.queue.size() == k) {
                if(this.queue.peek() < num) {
                    this.queue.poll();
                    this.queue.add(num);
                }
            } else {
                this.queue.add(num);
            }
        }
    }
    
    public int add(int val) {
        if(queue.size() == k) {
            if(val > queue.peek()) {
                queue.poll();
                queue.add(val);
            } 
        } else {
            queue.add(val);
        }
        return queue.peek();
    }
}
```

## 性能分析

&emsp;`add`方法时间复杂度为$O(\log_2k)$，构造方法时间复杂度为$O(N\log_2k)$。空间复杂度为$O(k)$。

执行用时：30ms，在所有java提交中击败了37.87%的用户。

内存消耗：57.1MB，在所有java提交中击败了14.10%的用户。

## 官方解题

&emsp;暂无，密切关注。