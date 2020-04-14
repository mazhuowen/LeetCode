[toc]

Given an array `A` of integers, for each integer `A[i]` we need to choose **either** `x = -K` or `x = K`, and add `x` to `A[i] (only once)`.

After this process, we have some array `B`.

Return the smallest possible difference between the maximum value of `B` and the minimum value of `B`.



**Note:**

* $1 \le \text{A.length} \le 10000$
* $0 \le \text{A[i]} \le 10000$
* $0 \le K \le 10000$



## 题目解读

&emsp;对数组中的每个元素选择添加或减少$K$，给出这样操作后数组中最大值和最小值的最小差距。

```java
class Solution {
    public int smallestRangeII(int[] A, int K) {

    }
}
```

## 程序设计

* 观察到当序列最大值最小值之差小于$K$时，序列最优方案是整体加$K$或减$K$；当序列之差大于等于$K$，则一部分点减少$K$，另一部分增加$K$，关键在于分隔点的选取，可以对序列排序，依次遍历尝试分隔方案，分隔点及之前的点都加$K$，序列区间为$A[0] + K \sim A[i] + K$，分隔点之后的点减$K$，序列区间为$A[i + 1] - K \sim A[N - 1] - K$，则选择这两个区间的并，计算区间差，最后在所有区间差中选择最小的即可。

```java
class Solution {
    public int smallestRangeII(int[] A, int K) {
        Arrays.sort(A);
        // 序列差值小于K，则整体加K或减K，最小差值就是最大值减去最小值
         int res =  A[A.length - 1] - A[0];
        if (K >= res) return res;

       // 序列差值大于等于K，则存在分隔点使得前面加K后面减K
        // 遍历序列，判断i是否是分隔点
        for (int i = 0; i < A.length - 1; i++) {
            // 最高点为分隔点加K和原最大值减K的较大值
            int max = Math.max(A[i] + K, A[A.length - 1] - K);
            // 最低点为分隔点后一点减K和原最小点加K的较小值
            int min = Math.min(A[i + 1] - K, A[0] + K);
            // 在所有可能的分隔点中选择最小的
            res = Math.min(res, max - min);
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：10ms，在所有java提交中击败了95.00%的用户。

内存消耗：40.5MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;见上。