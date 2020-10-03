[toc]

Given an array `nums` with $n$ integers, your task is to check if it could become non-decreasing by modifying **at most** $1$ element.

We define an array is non-decreasing if $\text{nums[i]} \le \text{nums[i + 1]}$ holds for every $i$ (0-based) such that ($0 \le i \le n - 2$).



**Constraints:**

- $1 \le n \le 10^4$
- $-10^5 \le \text{nums[i]} \le 10^5$



## 题目解读

&emsp;判断是否可最多修改一个元素使得数组非递减。

```java
class Solution {
    public boolean checkPossibility(int[] nums) {

    }
}
```

## 程序设计

* 首先问题似乎是查找数组中的山顶，然后判断将山顶修改后是否符合条件；但该逻辑存在缺陷，如`4,5,7,1,8`中修改的不能是山顶`7`，而是山谷`1`；
* 从而只需边离判断不符合条件的情况，即$\text{nums[i - 1]} > \text{nums[i]}$，此时需判断是修改$i - 1$的值还是修改$i$的值，有如下情况：
  * $i - 1 = 0$或$\text{nums[i - 2]} \le \text{nums[i]}$，则只需修改$i - 1$位置的值即可；
  * $i$为数组末或$\text{nums[i - 1]} \le nums[i + 1]$。则只需修改$i$位置的值；
* 根据题意只能最多修改一次，即需要判断是否出现多次不符合要求的值。

```java
class Solution {
    public boolean checkPossibility(int[] nums) {
        boolean modify = false;
        // 标记修改点
        for (int i = 1; i < nums.length; i++) {
            // 不符合要求
            if (nums[i - 1] > nums[i]) {
                // 存在多个不符合条件的情况
                if (modify) return false;
                // 判断是否可修改
                else {
                    // 若在数组首、尾则直接修改首尾数字；
                    // 否则判断是修改第一个数还是第二个数。
                    if (i - 1 == 0 || i == nums.length - 1 || nums[i - 2] <= nums[i] || nums[i - 1] <= nums[i + 1]) modify = true;
                    // 如果都不符合条件，说明无法得到正确结果
                    else return false;
                }
            }
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：40.2MB，在所有java提交中击败了22.92%的用户。

## 官方解题

&emsp;官方思路同上。