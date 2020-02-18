[toc]

Given an unsorted integer array, find the smallest missing positive integer.



**Note:**

Your algorithm should run in $O(N)$ time and uses constant extra space.



## 题目解读

&emsp;给定非排序数组，得到缺失的第一个正整数。题目限定时间复杂度为$O(N)$，空间复杂度为$O(1)$。

```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        
    }
}
```

## 程序设计

* 仔细观察数组，会发现缺失的第一个正整数不会超过数组长度`n+1`。可以将数组中小于等于0和大于`n`的数置为-1。
* 在置为-1后，需要将数组中正整数数字放到对应的索引下（数字减一）；此时需要注意替换的值也需要放到合适的位置。
* 在上述替换放置的过程中，还需要注意重复值的问题，如果存在重复值，则会发生死循环。

```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        // 缺失的最小正整数不会超过n+1
        int n = nums.length;
        // 将超过n、小于等于0的数置为-1
        for(int i = 0; i < nums.length; i++) {
            if(nums[i] <= 0 || nums[i] > n) {
                nums[i] = -1;
            }
        }
        // 将大于0的数num置与num-1的下标
        for(int i = 0; i < n; i++) {
            // 交换数值，同时保证交换的数值也会被放到正确的位置，故用while
            while(nums[i] != i + 1 && nums[i] > 0) {
                int temp = nums[nums[i] - 1];
                // 待交换的数值与交换的一致，表示数字重复，则不交换，当前位置置为-1
                if(temp == nums[i]) {
                    nums[i] = -1;
                } 
                // 交换
                else {
                    nums[nums[i] - 1] = nums[i];
                    nums[i] = temp;
                }
            }
        }
		// 遇到的第一个小于0的下标加一就是缺失的数
        for(int i = 0; i < n; i++) {
            if(nums[i] < 0) {
                return i + 1;
            }
        }
		// 遍历完没有找到-1，则表示数组是从1开始的连续数字，此时缺失值为n+1
        return n + 1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了98.14%的用户。

内存消耗：40.3MB，在所有java提交中击败了5.03%的用户。

## 官方解题

&emsp;官方解题思路一致，都是利用数组规律，处理不一样，官方将小于等于0及大于`n`的置为1，而将正整数放到相应位置并取反。这样遍历时遇到的第一个大于0的索引就是缺失值。