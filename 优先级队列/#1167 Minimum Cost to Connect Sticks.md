[toc]

You have some number of sticks with positive integer lengths. These lengths are given as an array `sticks`, where `sticks[i]` is the length of the `i`th stick.

You can connect any two sticks of lengths `x` and `y` into one stick by paying a cost of `x + y`. You must connect all the sticks until there is only one stick remaining.

Return the minimum cost of connecting all the given sticks into one stick in this way.



**Constraints:**

- $1 \le \text{sticks.length} \le 10^4$
- $1 \le \text{sticks[i]} \le 10^4$



## 题目解读

&emsp;合并棍子为一根，求最小的代价。

```java
class Solution {
    public int connectSticks(int[] sticks) {
        
    }
}
```

## 程序设计

* 可知从最小的棍子开始合并，代价较小，采用堆动态合并维护大小关系。

```java
class Solution {
    public int connectSticks(int[] sticks) {
        if (sticks == null || sticks.length <= 1) return 0;
		
        // 最小堆
        PriorityQueue<Integer> queue = new PriorityQueue<Integer>();
        for (int stick : sticks) queue.add(stick);

        // 动态合并
        int cost = 0;
        while (queue.size() > 1) {
            int newStick = queue.poll() + queue.poll();
            queue.add(newStick);
            cost += newStick;
        } 
        return cost;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：80ms，在所有java提交中击败了91.11%的用户。

内存消耗：40MB，在所有java提交中击败了22.95%的用户。

## 官方解题

&emsp;官方思路类似。