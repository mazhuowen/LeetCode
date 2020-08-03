[toc]

You are given an integer array `nums`. The value of this array is defined as the sum of $\vert \text{nums}[i]-\text{nums}[i+1]\vert$ for all $0 \le i < \text{nums.length}-1$.

You are allowed to select any subarray of the given array and reverse it. You can perform this operation **only once**.

Find maximum possible value of the final array.



**Constraints:**

- $1 \le \text{nums.length} \le 3*10^4$
- $-10^5 \le \text{nums}[i] \le 10^5$



## 题目解读

&emsp;规定数组的值为相邻数字差的绝对值之和，求反转子数组一次使得数组最大的值。

```java
class Solution {
    public int maxValueAfterReverse(int[] nums) {
        
    }
}
```

## 程序设计

* 首先分析可知，反转子数组只影响子数组边界的值，其他位置相邻数字差的绝对值不变；假设边界位置数字分别为$a$、$b$、$c$、$d$，$b \sim c$为待反转的子数组，则原先边界位置的值为$\vert a - b \vert + \vert c - d \vert$，反转后为$\vert a - c\vert + \vert b - d\vert$，如果根据这四个数字的大小拆分绝对值，则情况变得很复杂。
* 换个思路，考虑$a$、$c$组成的区间与$b$、$d$组成的区间的关系：
  * 如果$c$在$a$、$b$（即$[a,b]$或$[b,a]$）构成的区间内，即两个区间相交，则$\Delta = \vert a - c\vert + \vert b - d\vert - \vert a - b \vert - \vert c - d \vert$，$\vert a - b\vert = \vert a - c \vert + \vert c - b \vert$，$\vert b - d \vert = \vert b - c \vert + \vert c - d \vert$，带入得$\Delta = 0$，即区间相交的情况下反转的值不变；
  * 如果不相交，假设$c$、$d$是较大的区间，则$\vert a - c \vert = \vert a - b \vert + \vert b - c \vert$，$\vert b - d \vert = \vert b - c \vert + \vert c - d \vert$，带入得$\Delta = 2 * (c - b)$。
* 经上述分析可知，需要选择所有不相交区间中差值最大的情况，即较小区间中的较大值$b$和较大区间中的较小值$c$之差的最大值。
* 还需要考虑反转子数组是数组开头或结尾的情况。

```java
class Solution {
    public int maxValueAfterReverse(int[] nums) {
        int sum = 0, res = 0;
        int minMax = Integer.MIN_VALUE, maxMin = Integer.MAX_VALUE;
        for (int i = 0; i < nums.length - 1; i++) {
            sum += Math.abs(nums[i] - nums[i + 1]);
            minMax = Math.max(minMax, Math.min(nums[i], nums[i + 1]));
            maxMin = Math.min(maxMin, Math.max(nums[i], nums[i + 1]));
        }

        res = Math.max(sum, sum + 2 * (minMax - maxMin));
        // 边界情况
        for (int i = 1; i < nums.length - 1; i++) {
            // 交换0~i
            res = Math.max(res, sum + Math.abs(nums[0] - nums[i + 1]) - Math.abs(nums[i] - nums[i + 1]));
            // 交换i-1~len-1
            res = Math.max(res, sum + Math.abs(nums[nums.length - 1] - nums[i - 1]) - Math.abs(nums[i] - nums[i - 1]));
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：10ms，在所有java提交中击败了59.46%的用户。

内存消耗：40.1MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。