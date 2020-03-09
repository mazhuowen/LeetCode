[toc]

Given an array `nums`, we call $(i, j)$ an **important reverse pair** if $i < j$ and $\text{nums[i]} > 2*\text{nums[j]}$.

You need to return the number of important reverse pairs in the given array.

Note:

* The length of the given array will not exceed `50,000`.
* All the numbers in the input array are in the range of 32-bit integer.



## 题目解读

&emsp;给定数组的重要翻转对的数目。重要翻转对的定义为索引在前的数值大于在后的数值的两倍。

```java
class Solution {
    public int reversePairs(int[] nums) {

    }
}
```

## 程序设计

* 是[#315 Count of Smaller Numbers After Self](./#315 Count of Smaller Numbers After Self.md)逆序对的变形，只是这次必不能和归并步骤合在一起。

```java
class Solution {
    public int reversePairs(int[] nums) {
        if(nums == null || nums.length == 0) {
            return 0;
        }
        int count = mergeCount(nums, 0, nums.length - 1);
        return count;
    }
	// 归并排序
    private int mergeCount(int[] nums, int start, int end) {
        if(start >= end) {
            return 0;
        }
        int mid = start + (end - start) / 2;
        int count = mergeCount(nums, start, mid) + mergeCount(nums, mid + 1, end);
        // 计数
        count += merge(nums, start, mid, end);
        mergeSort(nums, start, mid, end);
        return count;
    }
	// 计数，将右半区间的值乘二与左半区间比较，转化为逆序对的问题
    private int merge(int[] nums, int start, int mid, int end) {
        int count = 0;
        int i = start, j = mid + 1;
        while(i <= mid && j <= end) {
            // 需注意溢出
            if(nums[i] <= 2 * (long)nums[j]) {
                count += j - mid - 1;
                i++;
            } else {
                j++;
            }
        }
        while (i <= mid) {
            i++;
            count += end - mid;
        }
        return count;
    }
	// 归并阶段
    private void mergeSort(int[] nums, int start, int mid, int end) {
        int[] temp = new int[end - start + 1];
        int i = start, j = mid + 1, idx = 0;
        while(i <= mid && j <= end) {
            if(nums[i] <= nums[j]) {
                temp[idx++] = nums[i++];
            } else {
                temp[idx++] = nums[j++];
            }
        }
        while (i <= mid) {
            temp[idx++] = nums[i++];
        }
        while (j <= end) {
            temp[idx++] = nums[j++];
        }
        for(idx = 0; idx < temp.length; idx++) {
            nums[start + idx] = temp[idx];
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：57ms，在所有java提交中击败了70.51%的用户。

内存消耗：53.5MB，在所有java提交中击败了21.57%的用户。

## 官方解题

&emsp;除了归并思路，官方还提供了树状数组的思路。