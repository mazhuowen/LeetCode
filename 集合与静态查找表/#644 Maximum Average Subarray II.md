[toc]

Given an array consisting of n integers, find the contiguous subarray whose length is **greater than or equal** to k that has the maximum average value. And you need to output the maximum average value.



**Note**:

* $1 \le k \le n \le 10000$.
* Elements of the given array will be in range `[-10,000, 10,000]`.
* The answer with the calculation error less than $10^{-5}$ will be accepted.



## 题目解读

&emsp;给定数组，找到子数组长度大于等于$k$且平均值最大，输出这个平均值。

```java
class Solution {
    public double findMaxAverage(int[] nums, int k) {

    }
}
```

## 程序设计

* 最简单的暴力法会报超时。

```java
class Solution {
    public double findMaxAverage(int[] nums, int k) {
        double res = Integer.MIN_VALUE;
        for(int i = 0; i <= nums.length - k; i++) {
            long sum = 0;
            for(int j = i; j < nums.length; j++) {
                sum += nums[j];
                // 长度大于等于k，则更新
                if(j - i + 1 >= k) {
                    res = Math.max(res, (double)sum / (j - i + 1));
                }
            }
        }
        return res;
    }
}
```

* 参考官方思路，可以使用二分来猜测平均值，然后验证是否存在，如此迭代直到均值误差符合要求。

```java
class Solution {
    public double findMaxAverage(int[] nums, int k) {
        double min = Integer.MAX_VALUE, max = Integer.MIN_VALUE;
        for(int num : nums) {
            min = Math.min(min, num);
            max = Math.max(max, num);
        }
        // 误差小于规定值后停止循环
        double error = Integer.MAX_VALUE; 
        // 记录上一次猜测值
        double pre = max;
        while(error > 0.00001) {
            // 二分猜测平均值
            double mid = (min + max) / 2;
            if(check(nums, mid, k)) {
                min = mid;
            } else {
                max = mid;
            }
            // 更新误差
            error = Math.abs(mid - pre);
            pre = mid;
        }
        return pre;
    }

    private boolean check(int[] nums, double guess, int k) {
        // 记录从0到i的元素和
        double sum = 0;
        for(int i = 0; i < k; i++) {
            sum += nums[i] - guess;
        }
        // 和大于0，说明平均值大于guess
        if(sum >= 0) {
            return true;
        }
        // 分别记录当前索引k个元素前到索引0的和、k个索引前到开头的最小和
        // 注意此处minPreSum初始化为0，不是最大值，因为当preKSum大于0时，只需要比较sum大于等于0
        // 当preKSum小于0时，才会比较sum到preKSum之间的子区间是否大于0
        double preKSum = 0, minPreSum = 0;
        for(int i = k; i < nums.length; i++) {
            sum += nums[i] - guess;
            preKSum += nums[i - k] - guess;
            minPreSum = Math.min(minPreSum, preKSum);
            // 表示当前索引i到之前最小索引的均值大于guess
            if(sum >= minPreSum) {
                return true;
            } 
        }
        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2(max - min))$，空间复杂度为$O(1)$。

执行用时：49ms，在所有java提交中击败了76.19%的用户。

内存消耗：42.7MB，在所有java提交中击败了5.26%的用户。

## 官方解题

&emsp;同上。