[toc]

Given an array `A` of positive integers, `A[i]` represents the value of the `i`-th sightseeing spot, and two sightseeing spots $i$ and $j$ have distance $j - i$ between them.

The score of a pair ($i < j$) of sightseeing spots is (`A[i] + A[j] + i - j`) : the sum of the values of the sightseeing spots, **minus** the distance between them.

Return the maximum score of a pair of sightseeing spots.



**Note:**

* $2 \le \text{A.length} \le 50000$
* $1 \le \text{A[i]} \le 1000$



## 题目解读

&emsp;求最大的`A[i] + A[j] + i - j`。

```java
class Solution {
    public int maxScoreSightseeingPair(int[] A) {

    }
}
```

## 程序设计

* 可分解为`A[i] + i`和`A[j] - j`，即记录之前位置的最大的`A[i] + i`，加上当前位置的`A[j] - j`。

```java
class Solution {
    public int maxScoreSightseeingPair(int[] A) {
        int max = 0, maxSum = A[0] + 0;
        for (int j = 1; j < A.length; j++) {
            max = Math.max(max, maxSum + A[j] - j);
            maxSum = Math.max(maxSum, A[j] + j);
        }
        return max;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：4ms，在所有java提交中击败了89.82%的用户。

内存消耗：47.8MB，在所有java提交中击败了9.02%的用户。

## 官方解题

&emsp;官方思路类似。