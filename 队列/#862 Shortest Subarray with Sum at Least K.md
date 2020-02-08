[toc]

Return the **length** of the shortest, non-empty, contiguous subarray of A with sum at least K.

If there is no non-empty subarray with sum at least K, return -1.



**Note:**

* $1 \le \text{A.length} \le 50000$
* $-10^5 \le \text{A[i]} \le 10^5$
* $1 \le \text{K} \le 10^9$



## 题目解读

&emsp;给定数组，需要找到最短子序列使得元素值之和至少为$K$。

```java
class Solution {
    public int shortestSubarray(int[] A, int K) {
        
    }
}
```

## 程序设计

* 使用前缀和的技巧，将问题转化为寻找`preSum[y] - preSum[x] >= K`且`y - x`最小的问题。可以使用队列来模拟，当队尾值和队首值满足上述条件，则记录更新，队首出队；当队尾值大于等于当前值，则队尾出队。最后当前值入队。

```java
class Solution {
    public int shortestSubarray(int[] A, int K) {
        if(A == null) {
            return -1;
        }
        // 记录前缀和
        int[] preSum = new int[A.length + 1];
        for(int i = 0; i < A.length; i++) {
            preSum[i + 1] = preSum[i] + A[i];
        }
        int len = Integer.MAX_VALUE;
        Deque<Integer> deque = new LinkedList<>();
        // 遍历入队
        for(int i = 0; i < preSum.length; i++) {
            // 队尾值大于等于当前值，队尾出队（保证队列单调递增）
            // 假设后续有一个值使得减去队尾值不小于K，而队尾值大于等于当前值，
            // 即后续那个值减去当前值也大于等于K，而当前值索引比队尾值大，意味着与后续值组成的序列更短
            while(!deque.isEmpty() && preSum[deque.getLast()] >= preSum[i]) {
                // 遍历移除队尾值
                deque.removeLast();
            }
            // 当前队列的值满足条件，则更新序列长度
            while(!deque.isEmpty() && preSum[i] - preSum[deque.getFirst()] >= K) {
                // 队首出队。队尾减去队首大于等于K，如果不出对假设后续有一个值减去队首大于等于K，但明显序列长度不是最小，故队首出队
                len = Math.min(len, i - deque.removeFirst());
            }
            // 入队当前值
            deque.add(i);
        }
        // 返回长度
        return len == Integer.MAX_VALUE ? -1 : len;
    }
}
```

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(N)$。

执行用时：30ms，在所有java提交中击败了76.40%的用户。

内存消耗：51.8MB，在所有java提交中击败了44.52%的用户。

## 官方解题

&emsp;上述代码为官方提供。

> 如果题目改为等于K的序列，则可以简化为如下代码：

```java
class Solution {
    public int shortestSubarray(int[] A, int K) {
        if(A == null) {
            return -1;
        }
        Queue<Integer> queue = new LinkedList<>();
        int sum = 0;
        int len = Integer.MAX_VALUE;
        for(int num : A) {
            // 队列之和大于等于K，而当前值大于零，则出队，入队当前值
            if(!queue.isEmpty() && sum >= K && num >= 0) {
                sum -= queue.poll();
            }
            // 队列和小于K或大于等于K但当前值小于零，直接入队
            queue.add(num);
            sum += num;
            // 满足条件，则更新
            if(sum == K) {
                len = Math.min(len, queue.size());
            }
        }
        return len == Integer.MAX_VALUE ? -1 : len;
    }
}
```