[toc]

Given an array of integers `nums`, find the maximum length of a subarray where the product of all its elements is positive.

A subarray of an array is a consecutive sequence of zero or more values taken out of that array.

Return the maximum length of a subarray with positive product.



**Constraints:**

- $1 \le \text{nums.length} \le 10^5$
- $-10^9 \le \text{nums[i]} \le 10^9$



## 题目解读

&emsp;给定数组，返回乘积是正数的最大子数组长度。

```java
class Solution {
    public int getMaxLen(int[] nums) {
        
    }
}
```

## 程序设计

* 使用双指针遍历，在遍历过程中记录队列中负数的数目，如果是偶数，则乘积是正数，否则为负数；当遇到$0$后，如此时队列中有奇数个负数，则需滑动左侧指针，直到有偶数个负数，更新并定位窗口到$0$的右侧重新滑动。

```java
class Solution {
    public int getMaxLen(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        
        int res = 0;
        // 左右指针，当前队列负数计数
        int left = 0, right = 0, count = 0;
        while (right < nums.length) {
            // 遇到0则首先滑动左端直到负数为奇数个，更新
            if (nums[right] == 0) {
                while (count % 2 == 1) {
                    if (nums[left++] < 0) count--;
                }
                res = Math.max(res, right - left);
                // 重定位到0的右端
                left = right + 1;
            }
            // 非0则判断是否小于0
            else {
                if (nums[right] < 0) count++;
                // 队列中有偶数个负数，乘积为正，更新
                if (count % 2 == 0) res = Math.max(res, right - left + 1);
            }
            right++;
        }
        
        // 更新最后一个队列（首先左端出队，直到队列中有偶数个负数）
        while (left < right && count % 2 == 1) if (nums[left++] < 0) count--;
        res = Math.max(res, right - left);
        
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：4ms，在所有java提交中击败了81.28%的用户。

内存消耗：54.7MB，在所有java提交中击败了75.19%的用户。

## 官方解题

&emsp;暂无，密切关注。