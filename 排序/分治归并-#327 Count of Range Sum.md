[toc]

Given an integer array `nums`, return the number of range sums that lie in `[lower, upper]` inclusive.
Range sum `S(i, j)` is defined as the sum of the elements in `nums` between indices $i$ and $j$ ($i \le j$), inclusive.



**Note:**
A naive algorithm of $O(n^2)$ is trivial. You MUST do better than that.



## 题目解读

&emsp;给定数组，返回区间和在给定范围内的数目。

```java
class Solution {
    public int countRangeSum(int[] nums, int lower, int upper) {
        
    }
}
```

## 程序设计

* 最基本的思路计算前缀和，然后两层循环遍历计算。

```java
class Solution {
    public int countRangeSum(int[] nums, int lower, int upper) {
        if(nums.length == 0) {
            return 0;
        }
        // 注意溢出，采用长整形
        long[] preSum = new long[nums.length + 1];
        for(int i = 0; i < nums.length; i++) {
            preSum[i + 1] = nums[i] + preSum[i];
        }
        int count = 0;
        for(int i = 1; i <= nums.length; i++) {
            for(int j = 0; j < i; j++) {
                // 采用长整形
                long sum = preSum[i] - preSum[j];
                if(sum >= lower && sum <= upper) {
                    count++;
                }
            }
        }
        return count;
    }
}
```

## 性能分析

&emsp;暴力法时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：190ms，在所有java提交中击败了8.48%的用户。

内存消耗：41MB，在所有java提交中击败了5.95%的用户。

## 官方解题

