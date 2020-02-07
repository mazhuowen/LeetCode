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



## 性能分析



## 官方解题



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

