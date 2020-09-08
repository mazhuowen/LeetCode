[toc]

Given an array `A` of non-negative integers, return the maximum sum of elements in two non-overlapping (contiguous) subarrays, which have lengths `L` and `M`.  (For clarification, the `L`-length subarray could occur before or after the `M`-length subarray.)

Formally, return the largest `V` for which `V = (A[i] + A[i+1] + ... + A[i+L-1]) + (A[j] + A[j+1] + ... + A[j+M-1])` and either:

* $0 \le i < i + L - 1 < j < j + M - 1 < \text{A.length}$, or
* $0 \le j < j + M - 1 < i < i + L - 1 < \text{A.length}$.



**Note:**

* $L \ge 1$
* $M \ge 1$
* $L + M \le \text{A.length} \le 1000$
* $0 \le \text{A[i]} \le 1000$



## 题目解读

&emsp;给定数组，求两段非重叠子数组最大和。

```java
class Solution {
    public int maxSumTwoNoOverlap(int[] A, int L, int M) {
        
    }
}
```

## 程序设计

* 

```java
class Solution {
    public int maxSumTwoNoOverlap(int[] A, int L, int M) {
        // 计算前缀和
        int[] preSum = new int[A.length + 1];
        for (int i = 1; i <= A.length; i++) {
            preSum[i] = preSum[i - 1] + A[i - 1];
        }
        
        // 最大值，窗口L的最大值，窗口M的最大值
        int max = 0;
        int LMax = preSum[L], MMax = preSum[M];
        // 滑动窗口，当前为i-M的窗口或i-L的窗口
        for (int i = L + M; i <= A.length; i++) {
            // 统计i-M前的最大L窗口
            LMax = Math.max(LMax, preSum[i - M] - preSum[i - M - L]);
            // 统计i-L前的最大M窗口
            MMax = Math.max(MMax, preSum[i - L] - preSum[i - M - L]);
            // 计算最大值
            max = Math.max(max, Math.max(LMax + preSum[i] - preSum[i - M], MMax + preSum[i] - preSum[i - L]));
        }
        return max;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.2MB，在所有java提交中击败了91.55%的用户。

## 官方解题

&emsp;暂无，密切关注。