[toc]

Given `n` balloons, indexed from `0` to `n-1`. Each balloon is painted with a number on it represented by array `nums`. You are asked to burst all the balloons. If the you burst balloon `i` you will get `nums[left] * nums[i] * nums[right]` coins. Here `left` and `right` are adjacent indices of `i`. After the burst, the `left` and `right` then becomes adjacent.

Find the maximum coins you can collect by bursting the balloons wisely.

Note:

* You may imagine `nums[-1] = nums[n] = 1`. They are not real therefore you can not burst them.
* $0 \le n \le 500$, $0 \le \text{nums[i]} \le 100$



## 题目解读

&emsp;有`n`个气球，编号为`0`到`n-1`，每个气球上都标有一个数字，这些数字存在数组`nums`中。现在要戳破所有的气球。每当戳破一个气球`i`时，可以获得`nums[left] * nums[i] * nums[right]`个硬币。 这里的`left`和`right`代表和`i`相邻的两个气球的序号。当戳破气球`i`后，气球`left`和气球`right`就变成了相邻的气球。求所能获得硬币的最大数量。

```java
class Solution {
    public int maxCoins(int[] nums) {

    }
}
```

## 程序设计



```java

```



## 性能分析



## 官方解题

