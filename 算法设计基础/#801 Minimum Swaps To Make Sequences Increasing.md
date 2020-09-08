[toc]

We have two integer sequences `A` and `B` of the same non-zero length.

We are allowed to swap elements `A[i]` and `B[i]`.  Note that both elements are in the same index position in their respective sequences.

At the end of some number of swaps, `A` and `B` are both strictly increasing.  (`A` sequence is strictly increasing if and only if $\text{A[0]} < \text{A[1]} < \text{A[2]} < \dots < \text{A[A.length - 1]}$.)

Given `A` and `B`, return the minimum number of swaps to make both sequences strictly increasing.  It is guaranteed that the given input always makes it possible.


**Note**:

* `A`, `B` are arrays with the same length, and that length will be in the range $[1, 1000]$.
* `A[i]`, `B[i]` are integer values in the range $[0, 2000]$.



## 题目解读

&emsp;给定数组，可交换两个数组的元素，求使得两个数组严格递增的最少交换数。

```java
class Solution {
    public int minSwap(int[] A, int[] B) {
        
    }
}
```

## 程序设计

* 参考官方解题，使用两个数组，保持和交换数组表示在保持或交换当前元素的情况下的最少操作数。

```java
class Solution {
    public int minSwap(int[] A, int[] B) {
        if (A == null || B == null || A.length != B.length) throw new IllegalArgumentException("invalid param");

        // 动态规划数组，表示第i个元素交换或不变的最少操作数
        int[] keep = new int[A.length], swap = new int[A.length];
        Arrays.fill(keep, Integer.MAX_VALUE);
        Arrays.fill(swap, Integer.MAX_VALUE);
        keep[0] = 0;
        // 交换第一个元素
        swap[0] = 1;

        for (int i = 1; i < A.length; i++) {
            // 当前元素满足条件
            if (A[i] > A[i - 1] && B[i] > B[i - 1]) {
                // 保留序列满足条件，不变
                keep[i] = keep[i - 1];
                // 交换序列需交换当前位置（由于前一个位置交换了）
                swap[i] = swap[i - 1] + 1;
            }

            // 可交换后满足条件
            if (A[i] > B[i - 1] && B[i] > A[i - 1]) {
                // 交换前一个元素的基础上保持当前元素不变
                keep[i] = Math.min(keep[i], swap[i - 1]);
                // 保持前一个元素的基础上交换当前元素
                swap[i] = Math.min(swap[i], keep[i - 1] + 1);
            }
        }
        return Math.min(keep[A.length - 1], swap[B.length - 1]);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了96.31%的用户。

内存消耗：39.6MB，在所有java提交中击败了96.74%的用户。

## 官方解题

&emsp;上述思路参考官方解题。