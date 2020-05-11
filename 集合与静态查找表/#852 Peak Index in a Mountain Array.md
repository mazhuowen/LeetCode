[toc]

Let's call an array `A` a mountain if the following properties hold:

* $\text{A.length} \ge 3$
* There exists some $0 < i < \text{A.length} - 1$ such that $\text{A[0]} < \text{A[1]} < \dots \text{A[i-1]} < \text{A[i]} > \text{A[i+1]} > \dots > \text{A[A.length - 1]}$

Given an array that is definitely a mountain, return any i such that $\text{A[0]} < \text{A[1]} < \dots < \text{A[i-1]} < \text{A[i]} > \text{A[i+1]} > \dots > \text{A[A.length - 1]}$.



Note:

* $3 \le \text{A.length} \le 10000$
* $0 \le \text{A[i]} \le 10^6$
* `A` is a mountain, as defined above.



## 题目解读

&emsp;寻找山顶位置。

```java
class Solution {
    public int peakIndexInMountainArray(int[] A) {

    }
}
```

## 程序设计

* 二分查找。

```java
class Solution {
    public int peakIndexInMountainArray(int[] A) {
        if (A == null || A.length < 3) throw new IllegalArgumentException("invalid param");

        int left = 0, right = A.length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            // 下坡，向右爬
            if (mid + 1 < A.length && A[mid] > A[mid + 1] || mid - 1 >= 0 && A[mid - 1] > A[mid]) {
                right = mid;
            } 
            // 上坡。向左爬
            else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.9MB，在所有java提交中击败了20.00%的用户。

## 官方解题

&emsp;同上。