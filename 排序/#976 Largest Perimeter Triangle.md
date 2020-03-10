[toc]

Given an array `A` of positive lengths, return the largest perimeter of a triangle with **non-zero area**, formed from 3 of these lengths.

If it is impossible to form any triangle of non-zero area, return 0.



**Note:**

* $3 \le \text{A.length} \le 10000$
* $1 \le \text{A[i]} \le 10^6$



## 题目解读

&emsp;给定边长数组，查找能组成三角形的最大周长，如果无法组成三角形，则返回0。

```java
class Solution {
    public int largestPerimeter(int[] A) {

    }
}
```

## 程序设计

* 最基本的思路是排序，然后从尾遍历，由于已经是有序的了，判断两个小的边长是否大于大的边长，大于则满足条件，返回即可；否则三个数都向前遍历。
* 满足条件的值必然是三个连续的数。假设不连续，则中间的边大于最小的边，可以和原先的组合组成更大的周长。

```java
class Solution {
    public int largestPerimeter(int[] A) {
        if(A == null || A.length < 3) {
            return 0;
        }
        Arrays.sort(A);
        int a = A.length - 3, b = A.length - 2, c = A.length - 1;
        while (a >= 0) {
            // 满足条件
            if(A[a] + A[b] > A[c]) {
               return A[a] + A[b] + A[c];
            }
            // 关键逻辑，不满足条件一起向前遍历
            else {
                a--;
                b--;
                c--;
            }
        }
        return 0;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：9ms，在所有java提交中击败了96.62%的用户。

内存消耗：42.8MB，在所有java提交中击败了5.13%的用户。

## 官方解题

&emsp;同上。