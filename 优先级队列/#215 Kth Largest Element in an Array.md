[toc]

Find the **k**th largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

**Note:**
You may assume k is always valid, $1 \le k \le \text{array's length}$.



## 题目解读

&emsp;给定未排序的数组，找到第k大的数。

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        
    }
}
```

## 程序设计

* 首先想到使用优先级队列，遍历数组并入队；如果队列中元素数大于k，则出队最小值，保留较大值；遍历结束后留在堆中的是k个最大数值，队首就是第k小值。

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> queue = new PriorityQueue<>();
        for(int num : nums) {
            queue.add(num);
            // 如果堆大于k，出队最小元素
            if(queue.size() > k) {
                queue.poll();
            }
        }
        // 遍历完成后，堆中有k个元素，顶部就是最小值，也就是第k小的值
        return queue.poll();
    }
}
```

## 性能分析

&emsp;时间复杂度遍历为$O(N)$，每次需要入队时间复杂度为$O(\log_2k)$，总的时间复杂度为$O(N\log_2k)$。空间复杂度为$O(k)$。

执行用时：9ms，在所有java提交中击败了52.45%的用户。

内存消耗：46.9MB，在所有java提交中击败了5.01%的用户。

## 官方解题

&emsp;官方除了堆的方法，还提供了借鉴快速排序的方法。