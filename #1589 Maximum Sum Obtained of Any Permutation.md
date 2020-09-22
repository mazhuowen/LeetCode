[toc]

We have an array of integers, `nums`, and an array of requests where `requests[i] = [starti, endi]`. The ith request asks for the sum of `nums[starti] + nums[starti + 1] + ... + nums[endi - 1] + nums[endi]`. Both `starti` and `endi` are 0-indexed.

Return the maximum total sum of all requests **among all permutations** of nums.

Since the answer may be too large, return it **modulo** $10^9 + 7$.



**Constraints**:

* $n == \text{nums.length}$
* $1 \le n \le 10^5$
* $0 \le \text{nums[i]} \le 10^5$
* $1 \le \text{requests.length} \le 10^5$
* $\text{requests[i].length} == 2$
* $0 \le \text{start}_i \le \text{end}_i < n$



## 题目解读

&emsp;给定数组，求数组所有排列组合中规定区间所有区间合之和最大值。

```java
class Solution {
    public int maxSumRangeQuery(int[] nums, int[][] requests) {

    }
}
```

## 程序设计

* 给定数组`[1,2,3,4,5,10]`，对于区间`[0,2],[1,3],[1,1]`，如果区间合最大，则这些相交区间分派最大值，具体的说根据区间的重叠数目分派最大值，如上述区间都包含$1$，分派最大值$10$，有两个区间包含$2$，分派次大值$5$，剩余$0$、$3$都只有一个区间，可分派剩余的较大值$4$、$3$，最后答案就是$3 * 10 + 2 * 5 + 4 + 3 = 47$。
* 根据上述思路，需要统计区间重叠的数目，可在区间长度数组上先标记区间，然后再计算区间间的值，最后从最大值开始分派。

```java
class Solution {
    private static final int MOD = 1_000_000_007;

    public int maxSumRangeQuery(int[] nums, int[][] requests) {
        int n = nums.length;
        // 排序
        Arrays.sort(nums);
        // 统计区间
        int[] intervels = new int[n + 1];
        for (int[] request : requests) {
            intervels[request[0]]++;
            intervels[request[1] + 1]--;
        }
        // 填充区间间的值
        for (int i = 1; i <= n; i++) {
            intervels[i] += intervels[i - 1];
        }
        // 从最大值开始分派
        Arrays.sort(intervels);
        int res = 0;
        for (int i = n; i >= 1 && intervels[i] > 0; i--) {
            res = (res + intervels[i] * nums[i - 1]) % MOD;
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。



## 官方解题

&emsp;