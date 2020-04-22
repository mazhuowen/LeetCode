[toc]

Given an integer array `nums`, find the contiguous subarray within an array (containing at least one number) which has the largest product.



## 题目解读

&emsp;给定数组，返回连续乘积最大子序列的乘积。

```java
class Solution {
    public int maxProduct(int[] nums) {

    }
}
```

## 程序设计

* 参考社区思路，分别记录截止当前位置的最大、最小连续乘积，则当前最大、最小乘积与前一位置息息相关，要么在前一序列的基础上加入，要么从新开始序列。

```java
class Solution {
    public int maxProduct(int[] nums) {
        if (nums == null || nums.length == 0) return 0;

        int n = nums.length;
        int max[] = new int[n], min[] = new int[n];
        int res = max[0] = min[0] = nums[0];

        for (int i = 1; i < n; i++) {
            // 假设当前为正数，之前最大连续乘积为正数，则当前最大乘积为两者的乘积；
            // 之前最大连续乘积为负数，则说明前一个元素为负数，当前最大连续乘积需要重新从当前开始；当前为负数，同理。
            if (nums[i] < 0) {
                max[i] = Math.max(min[i - 1] * nums[i], nums[i]);
                min[i] = Math.min(max[i - 1] * nums[i], nums[i]);
            } else {
                max[i] = Math.max(max[i - 1] * nums[i], nums[i]);
                min[i] = Math.min(min[i - 1] * nums[i], nums[i]);
            }
            // 选择子序列中最大的乘积
            res = Math.max(res, max[i]);
        }
        return res;
    }
}
```

* 事实上只需要前一位置的最大最小值，不需要数组，可简化为：

```java
class Solution {
    public int maxProduct(int[] nums) {
        if (nums == null || nums.length == 0) return 0;

        int n = nums.length;
        int max, min, res;
        res = max = min = nums[0];

        for (int i = 1; i < n; i++) {
            int newMax, newMin;
            if (nums[i] < 0) {
                newMax = Math.max(min * nums[i], nums[i]);
                newMin = Math.min(max * nums[i], nums[i]);
            } else {
                newMax = Math.max(max * nums[i], nums[i]);
                newMin = Math.min(min * nums[i], nums[i]);
            }
            max = newMax; min = newMin;
            res = Math.max(res, max);
        }
        return res;
    }
}
```

* 不考虑负数，只考虑非负数最大连续乘积，由于是整数，最大连续乘积必然是不包含0的序列的乘积，这样可以从左往右遍历并累计乘积，直到遇到0则重新开始，每次都记录并更新最大值。当加入负数后，如果序列中有偶数个负数，结果仍然是序列乘积；如果序列中负数有奇数个，则该奇数将序列拆分为两部分，但是不能遇到负数就重新开始，因为最大乘积必然是序列中第一个奇数的右侧序列或最后一个奇数的左侧序列，无法决定哪个是最大的，无法决定重新开始位置。
* 可以再次反向遍历数组判断更新，这样正向判断奇数负数最后一个奇数，反向判断奇数负数第一个负数，结合得到的就是最大值。

```java
class Solution {
    public int maxProduct(int[] nums) {
        if (nums == null || nums.length == 0) return 0;

        int n = nums.length;
        int max = Integer.MIN_VALUE, cur = 1;
        for (int i = 0; i < n; i++) {
            cur *= nums[i];
            max = Math.max(max, cur);
            // 遇到0则重新开始
            if (cur == 0) cur = 1;
        }

        cur = 1;
        for (int i = n - 1; i >= 0; i--) {
            cur *= nums[i];
            max = Math.max(max, cur);
            if (cur == 0) cur = 1;
        }

        return max;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$，优化后为$O(1)$。

执行用时：2ms，在所有java提交中击败了92.68%的用户。

内存消耗：39.6MB，在所有java提交中击败了6.67%的用户。

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.7MB，在所有java提交中击败了6.67%的用户。

## 官方解题

&emsp;暂无，密切关注。