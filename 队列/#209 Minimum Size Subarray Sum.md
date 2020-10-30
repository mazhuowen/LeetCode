[toc]

Given an array of n positive integers and a positive integer s, find the minimal length of a **contiguous** subarray of which the sum ≥ s. If there isn't one, return 0 instead.



**Follow up**:
If you have figured out the $O(n)$ solution, try coding another solution of which the time complexity is $O(n\log_2n)$. 



## 题目解读

&emsp;找出数组中连续数字之和大于等于给定正整数的最小长度，没有则返回0。注意数组中数字都是正数。

```java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        
    }
}
```

## 程序设计

* 由于数组中数字都是正数，且给定的目标也是正数，遍历求和的过程实际上是队列入队出队的问题：当前队列和小于给定数字，则入队，当前大于等于给定数字，则出队再入队。

```java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int len = Integer.MAX_VALUE;
        int start = 0, sum = 0;
        for(int i = 0; i < nums.length; i++) {
            // 入队
            sum += nums[i];
            // 达到要求，更新并出队（由于后续还是正数，不出队长度比当前大，肯定不是最短长度）
            while(sum >= s) {
                len = Math.min(len, i - start + 1);
                // 出队
                sum -= nums[start++];
            }
        }
        return len == Integer.MAX_VALUE ? 0 : len;
    }
}
```

* 题目提到了实现$O(N\log_2N)$的方法，可以使用前缀和将无序正整数数组转化为有序数组，这样可以使用二分查找。

```java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        if(nums.length == 0) {
            return 0;
        }
        // 记录前缀和
        int[] sum = new int[nums.length + 1];
        for(int i = 0; i < nums.length; i++) {
            sum[i + 1] = sum[i] + nums[i];
        }

        int len = Integer.MAX_VALUE;
        for(int i = 0; i < sum.length; i++) {
            // 目标
            int target = sum[i] + s;
            // 查找范围
            int left = i + 1, right = sum.length - 1;
            while(left < right) {
                int mid = (left + right) / 2;
                // 找到则更新结果（本轮不会有比len更小的，结束查找）
                if(sum[mid] == target) {
                    len = Math.min(len, mid - i);
                    break;
                }
                if(sum[mid] < target) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            }
            // right为大于等于target的边界，更新
            if(sum[right] >= target)
                len = Math.min(len, right - i);
        }
        return len == Integer.MAX_VALUE ? 0 : len;
    }
}
```

## 性能分析

&emsp;双指针时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了89.98%的用户。

内存消耗：42.9MB，在所有java提交中击败了5.04%的用户。

&emsp;二分查找时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：6ms，在所有java提交中击败了25.55%的用户。

内存消耗：43.1MB，在所有java提交中击败了5.04%的用户。

## 官方解题

&emsp;官方提供最基础的暴力法，三层层循环计算所有连续元素的和，并更新长度。