[toc]

Given two lists of **closed** intervals, each list of intervals is pairwise disjoint and in sorted order.

Return the intersection of these two interval lists.

(Formally, a closed interval `[a, b]` (with $a \le b$) denotes the set of real numbers $x$ with $a \le x \le b$.  The intersection of two closed intervals is a set of real numbers that is either empty, or can be represented as a closed interval.  For example, the intersection of `[1, 3]` and `[2, 4]` is `[2, 3]`.)



Note:

* $0 \le \text{A.length} < 1000$
* $0 \le \text{B.length} < 1000$
* $0 \le \text{A[i].start, A[i].end, B[i].start, B[i].end} < 10^9$



## 题目解读

&emsp;给定两个区间数组，求区间的交集。

```java
class Solution {
    public int[][] intervalIntersection(int[][] A, int[][] B) {

    }
}
```

## 程序设计

* 类似于归并排序，同时遍历两个数组比较迭代。

```java
class Solution {
    public int[][] intervalIntersection(int[][] A, int[][] B) {
        if (A == null || A.length == 0 || B == null || B.length == 0) return new int[0][2];

        int i = 0, j = 0;
        List<int[]> res = new LinkedList<>();
        while (i < A.length && j < B.length) {
            int[] a = A[i], b = B[j];
            // 无相交，a在前
            if (a[1] < b[0]) i++;
            // 无相交，b在前
            else if (a[0] > b[1]) j++;
            // 相交
            else {
                // 选择最小的区间
                res.add(new int[]{Math.max(a[0], b[0]), Math.min(a[1], b[1])});
                // 结束区间在前的迭代
                if (a[1] <= b[1]) i++;
                else j++;
            }
        }
        return res.toArray(new int[0][2]);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(M + N)$，空间复杂度为$O(M + N)$。

执行用时：4ms，在所有java提交中击败了92.62%的用户。

内存消耗：41MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方思路类似，实现更为简洁。

```java
class Solution {
    public int[][] intervalIntersection(int[][] A, int[][] B) {
        List<int[]> res = new ArrayList<>();
        int i = 0, j = 0;
        while(i < A.length && j < B.length){
            int low = Math.max(A[i][0], B[j][0]);
            int high = Math.min(A[i][1], B[j][1]);
            // 相交
            if(low <= high){
                res.add(new int[]{low, high});
            }
            // 根据结束区间迭代
            if(A[i][1] < B[j][1]){
                i++;
            }else{
                j++;
            }
        }
        return res.toArray(new int[res.size()][]);
    }
}
```