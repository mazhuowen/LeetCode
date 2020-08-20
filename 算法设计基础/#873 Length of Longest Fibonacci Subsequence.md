[toc]

A sequence $X_1, X_2, \dots, X_n$ is fibonacci-like if:

* $n \ge 3$
* $X_i + X_{i+1} = X_{i+2}$ for all $i + 2 \le n$

Given a **strictly increasing** array `A` of positive integers forming a sequence, find the **length** of the longest fibonacci-like subsequence of `A`.  If one does not exist, return $0$.

(Recall that a subsequence is derived from another sequence `A` by deleting any number of elements (including none) from `A`, without changing the order of the remaining elements.  For example, `[3, 5, 8]` is a subsequence of `[3, 4, 5, 6, 7, 8]`.)



**Note**:

* $3 \le \text{A.length} \le 1000$
* $1 \le \text{A[0]} < \text{A[1]} < \dots < \text{A[A.length - 1]} \le 10^9$



## 题目解读

&emsp;给定严格单调递增序列，求最长的斐波那契子序列长度。

```java
class Solution {
    public int lenLongestFibSubseq(int[] A) {

    }
}
```

## 程序设计

* 斐波那契数列由起始的两个数字决定，最基本的可暴力遍历起始的两个数字，然后根据起始两个数字生成后续数字并判断是否在给定数组中存在。
* 参考社区思路，动态规划数组`dp(i,j)`表示结尾数字为`A[i]`和`A[j]`的数列最大长度，遍历到$i$，只需遍历前面所有组合使得两数之和等于`A[i]`，由于是严格递增，可采用双指针来判断遍历。

```java
class Solution {
    public int lenLongestFibSubseq(int[] A) {
        // 动态规划数组表示结尾数字索引为i，j所决定的数列最大长度
        int[][] dp = new int[A.length][A.length];
        for (int[] array : dp) Arrays.fill(array, 2);

        int res = 0;
        for (int i = 1; i < A.length; i++) {
            // 双指针，向前扩展
            int left = 0, right = i - 1;
            while (left < right) {
                // 构成数列
                if (A[left] + A[right] == A[i]) {
                    dp[right][i] = Math.max(dp[right][i], dp[left][right] + 1);
                    res = Math.max(res, dp[right][i]);
                    left++;
                    right--;
                } else if (A[left] + A[right] < A[i]) left++;
                else right--;
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：31ms，在所有java提交中击败了97.54%的用户。

内存消耗：49MB，在所有java提交中击败了10.28%的用户。

## 官方解题

&emsp;官方思路类似。