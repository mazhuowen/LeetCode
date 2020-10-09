[toc]

Given a binary array, find the maximum number of consecutive 1s in this array.



**Note**:

* The input array will only contain $0$ and $1$.
* The length of input array is a positive integer and will not exceed $10000$



## 题目解读

&emsp;查找数组中最长的连续的$1$。

```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        
    }
}
```

## 程序设计

* 遍历即可。

```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        if (nums == null || nums.length == 0) return 0;

        int max = 0, pre = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != 1) {
                max = Math.max(max, i - pre);
                pre = i + 1;
            }
        }
        max = Math.max(max, nums.length - pre);
        return max;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了94.81%的用户。

内存消耗：40.5MB，在所有java提交中击败了56.42%的用户。

## 官方解题

&emsp;官方思路类似。