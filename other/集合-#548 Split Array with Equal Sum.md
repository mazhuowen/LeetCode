[toc]

Given an array with n integers, you need to find if there are triplets (i, j, k) which satisfies following conditions:

* $0 < i$, $i + 1 < j$, $j + 1 < k < n - 1$
* Sum of subarrays $(0, i - 1)$, $(i + 1, j - 1)$, $(j + 1, k - 1)$ and $(k + 1, n - 1)$ should be equal.

where we define that subarray $(L, R)$ represents a slice of the original array starting from the element indexed L to the element indexed R.

Note:

* $1 \le n \le 2000$.
* Elements in the given array will be in range `[-1,000,000, 1,000,000]`.



## 题目解读

&emsp;给定数组找到三元组索引$(i,j,k)$，其中$i$、$k$不能是头尾位置，$j$不能是$i$的后继和$k$的前驱，其子区间元素之和相等。

```java
class Solution {
    public boolean splitArray(int[] nums) {

    }
}
```

## 程序设计



```java

```

## 性能分析



## 官方解题

