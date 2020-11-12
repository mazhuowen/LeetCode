[toc]

Given an integer array `arr` and an integer $k$, modify the array by repeating it $k$ times.

For example, if `arr = [1, 2]` and $k = 3$ then the modified array will be `[1, 2, 1, 2, 1, 2]`.

Return the maximum sub-array sum in the modified array. Note that the length of the sub-array can be $0$ and its sum in that case is $0$.

As the answer can be very large, return the answer **modulo** $10^9 + 7$.



**Constraints:**

- $1 \le \text{arr.length} \le 10^5$
- $1 \le k \le 10^5$
- $-10^4 \le \text{arr[i]} \le 10^4$



## 题目解读

&emsp;将数组复制$k$次，求最大的子数组和。

```java
class Solution {
    public int kConcatenationMaxSum(int[] arr, int k) {

    }
}
```

## 程序设计

* 不考虑重复数组，此时求最大子数组只需记录之前的最小前缀和，用当前的前缀和减去，统计最大子数组和即可；
* 考虑重复数组$k$次，此时：
  * 如果单个数组数组和大于$0$，则显然除了首尾，中间数组越多越好；对于第一个数组，需要选择当前位置到数组尾数组和最大的位置作为起始位置，对于最后一个数组，需要选择当前位置到数组头数组和最大的位置作为结束位置；
  * 如果单个数组数组和小于等于$0$，则其最大子数组和必然在单个数组中，又分为两种情况，即首尾连接、在数组中；

```java
class Solution {
    private final static int MOD = 1_000_000_007;

    public int kConcatenationMaxSum(int[] arr, int k) {
        long sum = 0;
        for (int num : arr) sum += num;
        // 数组和大于0且k大于1，则只需遍历首尾开始的最大数组和
        if (sum > 0 && k > 1) {
            // 从起始、结束位置开始的最大数组和
            long preMax = Integer.MIN_VALUE, postMax = Integer.MIN_VALUE;
            // 从起始、结束位置开始的数组和
            long pre = 0, post = 0;
            for (int i = 0; i < arr.length; i++) {
                // 计算前缀和
                pre += arr[i];
                post += arr[arr.length - 1 - i];
                // 统计
                preMax = Math.max(preMax, pre);
                postMax = Math.max(postMax, post);
            }
            return (int)((preMax + postMax + sum * (k - 2)) % MOD);
        } 
        // 数组和小于等于0或大于0但k为1，即最大子数组和必然在一个数组周期内
        else {
            // 查找连续循环子数组最大和
            long max = 0, preSum = 0;
            // 记录前缀和的最大值和最小值
            long preMin = 0, preMax = 0;
            for (int num : arr) {
                preSum += num;
                // 判断中间连续一段或首尾相连
                max = Math.max(max, Math.max(preSum - preMin, preMax + sum - preSum));
                preMin = Math.min(preMin, preSum);
                preMax = Math.max(preMax, preSum);
            }
            return (int)(max % MOD);
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：3ms，在所有java提交中击败了97.07%的用户。

内存消耗：51.4MB，在所有java提交中击败了73.33%的用户。

## 官方解题

&emsp;暂无，密切关注。