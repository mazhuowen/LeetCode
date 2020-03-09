[toc]

Given an array `nums`, we call $(i, j)$ an **important reverse pair** if $i < j$ and $\text{nums[i]} > 2*\text{nums[j]}$.

You need to return the number of important reverse pairs in the given array.

Note:

* The length of the given array will not exceed `50,000`.
* All the numbers in the input array are in the range of 32-bit integer.



## 题目解读

&emsp;给定数组的重要翻转对的数目。

```java
class Solution {
    public int reversePairs(int[] nums) {

    }
}
```

## 程序设计



```java
class Solution {
    public int reversePairs(int[] nums) {
        if(nums == null || nums.length == 0) {
            return 0;
        }
        int count = mergeCount(nums, 0, nums.length - 1);
        return count;
    }

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

    private int merge(int[] nums, int start, int mid, int end) {
        int count = 0;
        int i = start, j = mid + 1;
        while(i <= mid && j <= end) {
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



## 官方解题

