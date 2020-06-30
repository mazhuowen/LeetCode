[toc]

Given an array `A` of positive integers, call a (contiguous, not necessarily distinct) subarray of `A` good if the number of different integers in that subarray is exactly `K`.

(For example, `[1,2,3,1,2]` has `3` different integers: `1`, `2`, and `3`.)

Return the number of good subarrays of `A`.




Note:

* $1 \le \text{A.length} \le 20000$
* $1 \le \text{A[i]} \le \text{A.length}$
* $1 \le K \le \text{A.length}$



## 题目解读

&emsp;给定一个正整数数组`A`，如果`A`的某个子数组中不同整数的个数恰好为`K`，则称`A`的这个连续、不一定独立的子数组为好子数组。求好子数组的数目。

```java
class Solution {
    public int subarraysWithKDistinct(int[] A, int K) {

    }
}
```

## 程序设计

* 对于某个满足条件的最小区间，其前后若有区间内的元素，计数分别为`pre`、`next`，则以最小区间为基础组成的子数组数目为`pre * next`；这样需要维护队列，当计数达到要求值时，左边收缩边界，使得区间满足要求且最小，并计数`pre`；得到最小区间后，需要扩展计数右边界，计数`next`，这样可得到当前最小区间组合数。
* 在得到当前区间组合数后，左侧继续移动，使得当前区间不满足要求，继续下次迭代，重复上述步骤。

```java
class Solution {
    public int subarraysWithKDistinct(int[] A, int K) {
        if (A == null || A.length < K) return 0;

        int count = 0;
        // 区间
        int left = 0, right = 0;
        // 符合条件的最小区间前后包含元素数目
        int pre = 1, next = 1;
        // 计数
        Map<Integer, Integer> counter = new HashMap<>();
        while (right < A.length) {
            // 计数加一
            counter.put(A[right], counter.getOrDefault(A[right], 0) + 1);
            right++;
            // 队列中数值等于K个
            if (counter.size() == K) {
                // 收缩到最小的范围，并计数pre
                while (counter.get(A[left]) > 1) {
                    counter.put(A[left], counter.getOrDefault(A[left], 0) - 1);
                    left++;
                    pre++;
                }
                // 计数后续包含数值，注意最小区间不能移动
                int temp = right;
                while (temp < A.length && counter.containsKey(A[temp])) {
                    temp++;
                    next++;
                }
                // 以当前最小区间为基础的子数组数目
                count += pre * next;
                // 最小区间移除最左端元素，继续迭代更新
                counter.remove(A[left++]);
                // 重置前后数目
                pre = 1;
                next = 1;
            } 
        }

        return count;
    }
}
```

* 由于题目规定$1 \le \text{A[i]} \le \text{A.length}$，可利用数组来计数；其次上述思路每次都统计最小区间前后的组合数目，实际上可以只统计前面的数目，之后的部分在迭代到窗口时会统计前面的部分。

```java
class Solution {
    public int subarraysWithKDistinct(int[] A, int K) {
        if (A == null || A.length < K) return 0;

        int count = 0;
        // 区间
        int left = 0, right = 0;
        // 符合条件的最小区间前包含元素数目
        int pre = 1;
        // 计数
        int num = 0;
        int[] counter = new int[A.length + 1];
        while (right < A.length) {
            if (counter[A[right++]]++ == 0) num++;

            while (num > K) {
                if (--counter[A[left++]] == 0) {
                    num--;
                    // 区间新增数字，重置计数
                    pre = 1;
                }
            }

            if (num == K) {
                while (counter[A[left]] > 1) {
                    counter[A[left++]]--;
                    pre++;
                }
                // 如果后续元素都是区间内值，无新增值，则pre继续叠加
                count += pre;
            }
        }

        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度最好为$O(N)$，最坏为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：40ms，在所有java提交中击败了76.79%的用户。

内存消耗：43.3MB，在所有java提交中击败了100.00%的用户。

&emsp;优化后时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了100.00%的用户。

内存消耗：43.2MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方提供了使用双窗口的思路，非最优。