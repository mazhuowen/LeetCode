[toc]

You are standing at position `0` on an infinite number line. There is a goal at position `target`.

On each move, you can either go left or right. During the n-th move (starting from 1), you take n steps.

Return the minimum number of steps required to reach the destination.



**Note:**

`target` will be a non-zero integer in the range $[-10^9, 10^9]$.



## 题目解读

&emsp;从起始点`0`出发，求到达`target`的最少步数；第$n$步可选择向右或向左走$n$步。

```java
class Solution {
    public int reachNumber(int target) {
        
    }
}
```

## 程序设计

* 首先由于每一步方向可选左右，正负目的地具有对称性，可将负数转化为正数统一处理；如果能在一个方向前进，$n$步可到达$1 + 2 + \dots + n = \frac{n * (n + 1)}{2}$。
* 逆向思维假设最少需要$n$步到达`target`，其中有若干步是反向行走，此处假设为$i$，则$1 + 2 + \dots - i + \dots + n = 1 + 2 + \dots + i - 2 * i + \dots + n$，即$\frac{n * (n + 1)}{2} - 2 * \sum_{i \in \text{逆向}}i = \text{target}$，仔细观察上式，减去的项是偶数，可遍历$n$，第一个满足$\frac{n * (n + 1)}{2} - \text{target}$为偶数的$n$就是答案。

```java
class Solution {
    public int reachNumber(int target) {
        if (target < 0) target = -target;
		// 快速求n
        int n = (int)(Math.sqrt(2 * target + 0.25) - 0.5);
        // 迭代n到满足条件
        while (calu(n) < target || (calu(n) - target) % 2 == 1) n++;
        return n;
    }

    private int calu(int n) {
        return n * (n + 1) / 2;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.3MB，在所有java提交中击败了57.14%的用户。

## 官方解题

&emsp;官方思路大致类似。