[toc]

Implement **next permutation**, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be in-place and use only constant extra memory.



## 题目解读

&emsp;给定数组代表一个整数，输出紧跟当前字典序后的一下个整数。以`158476531`为例，下一个数是`158513467`。题目限定对于降序数字已达到最大，返回升序数字，也就是最小值。

```java
class Solution {
    public void nextPermutation(int[] nums) {
        
    }
}
```

## 程序设计

* 由于数组左边代表高位，右边代表低位，首先升序数字代表最小值，降序数字代表最大值。如果要找到当前数的下一个数，则从右边遍历，如果是升序，说明遍历的子序列是降序，没有组合可以比这个子序列大；直到遍历到一个数值比后继数值小，这个点`x`就是需要重新组合的地方；从`x`后遍历找到大于`x`的最小值（由于子序列是降序，可以从右遍历找到第一个大于`x`的数），交换位置。此时得到的数比原先的数大，但不是最小的比原先数大的数，还需将降序子序列转化为升序子序列。
* 对于已经是降序的数组，表示是最大数字。根据题目要求，需要反转数组。

<img src="../images/#31.gif" style="zoom:80%;" />

```java
class Solution {
    public void nextPermutation(int[] nums) {
        int replace = nums.length - 2;
        // 从右遍历，找到子序列不是降序的位置
        while(replace >= 0 && nums[replace] >= nums[replace + 1]) {
            replace--;
        }
        // 从右遍历，找到大于replace位置的最小值，并交换
        if(replace >= 0) {
            int index = nums .length - 1;
            while(index > replace && nums[index] <= nums[replace]) {
                index--;
            }
            // 交换位置
            int temp = nums[index];
            nums[index] = nums[replace];
            nums[replace] = temp;
        }
        // 反转replace后的数组（包含整个数组都是降序的情况）
        int start = replace + 1, end = nums.length - 1;
        while(start < end) {
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了99.92%的用户。

内存消耗：43.3MB，在所有java提交中击败了5.08%的用户。

## 官方解题

&emsp;上述思路参考官方解题。

