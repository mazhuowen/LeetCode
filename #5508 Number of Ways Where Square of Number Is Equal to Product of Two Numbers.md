[toc]

Given two arrays of integers `nums1` and `nums2`, return the number of triplets formed (type 1 and type 2) under the following rules:

* Type 1: Triplet (i, j, k) if $\text{nums1[i]}^2 == \text{nums2[j]} * \text{nums2[k]}$ where $0 \le i < \text{nums1.length}$ and $0 \le j < k < \text{nums2.length}$.
* Type 2: Triplet (i, j, k) if $\text{nums2[i]}^2 == \text{nums1[j]} * \text{nums1[k]}$ where $0 \le i < \text{nums2.length}$ and $0 \le j < k < \text{nums1.length}$.



**Constraints**:

* $1 \le \text{nums1.length, nums2.length} \le 1000$
* $1 \le \text{nums1[i], nums2[i]} \le 10^5$



## 题目解读

&emsp;给定两个数组，根据要求求符合条件的组合数。

```java
class Solution {
    public int numTriplets(int[] nums1, int[] nums2) {

    }
}
```

## 程序设计

* 最基本的，使用哈希表保存数组数字两两组合的乘积和计数，然后遍历另一个数组，计算得到结果。

```java
class Solution {
    public int numTriplets(int[] nums1, int[] nums2) {
        return helper(nums1, nums2) + helper(nums2, nums1);
    }

    private int helper(int[] nums1, int[] nums2) {
        int res = 0;
        if (nums2.length < 2) return res;
        // 统计数组中的乘积和计数
        Map<Long, Integer> counter = new HashMap<>();
        for (int i = 0; i < nums2.length; i++) {
            for (int j = i + 1; j < nums2.length; j++) {
                long prod = (long)nums2[i] * nums2[j];
                counter.put(prod, counter.getOrDefault(prod, 0) + 1);
            }
        }

        for (int i = 0; i < nums1.length; i++) {
            long target = (long)nums1[i] * nums1[i];
            res += counter.getOrDefault(target, 0);
        }

        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN\max(M,N))$，空间复杂度为$O(\max(M^2,N^2))$。



## 官方解题

&emsp;