[toc]

Given an array `nums`, write a function to move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.



Note:

* You must do this in-place without making a copy of the array.
* Minimize the total number of operations.



## 题目解读

&emsp;将0移动到数组末尾。

```java
class Solution {
    public void moveZeroes(int[] nums) {

    }
}
```

## 程序设计

* 交换首先想到冒泡排序，将0一直交换到末尾，时间复杂度为$O(N^2)$。

```java
class Solution {
    public void moveZeroes(int[] nums) {
        if (nums == null) throw new IllegalArgumentException("invalid param");

        int n = nums.length;
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < n - i; j++) {
                if (nums[j] == 0) swap(nums, j, j + 1);
            }
        }
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

* 可以直接移位，将非零值移动填充，最后在末尾补上0。

```java
class Solution {
    public void moveZeroes(int[] nums) {
         if (nums == null) throw new IllegalArgumentException("invalid param");

         int count = 0;
         int idx = 0;
         for (int num : nums) {
             if (num == 0) count++;
             else nums[idx++] = num;
         }

         for (int i = 0; i < count; i++) {
             nums[nums.length - i - 1] = 0;
         }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：40MB，在所有java提交中击败了5.62%的用户。

##  官方解题

&emsp;同上。