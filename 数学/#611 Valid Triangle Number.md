[toc]

Given an array consists of non-negative integers, your task is to count the number of triplets chosen from the array that can make triangles if we take them as side lengths of a triangle.



Note:

* The length of the given array won't exceed `1000`.
* The integers in the given array are in the range of `[0, 1000]`.



## 题目解读

&emsp;给定数组，寻找可以组成三角形的数目。

```java
class Solution {
    public int triangleNumber(int[] nums) {

    }
}
```

## 程序设计

* 首先想到的是排序数组，遍历选定第一条边，然后遍历选定第二条边，从后续的边中二分查找符合条件的边的区间。

```java
class Solution {

    public int triangleNumber(int[] nums) {
        if (nums == null || nums.length < 3) return 0;

        int count = 0;
        Arrays.sort(nums);
        for (int i = 0; i < nums.length - 2; i++) {
            if (nums[i] == 0) continue;

            for (int j = i + 1; j < nums.length - 1; j++) {
                int sum = nums[i] + nums[j];
                int diff = nums[j] - nums[i];

                int leftIdx = boundarySearch(nums, j + 1, nums.length, diff, false);
                if (leftIdx == nums.length) break;

                int rightIdx = boundarySearch(nums, j + 1, nums.length, sum, true);
                if (rightIdx < leftIdx) continue;
                count += rightIdx - leftIdx;
            }
        }
        return count;
    }

    // flag表示是否是右边界
    private int boundarySearch(int[] nums, int start, int end, int target, boolean flag) {
        int left = start, right = end;
        while (left < right) {
            int mid = left + (right - left) / 2;

            if (nums[mid] > target || (flag && nums[mid] == target)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```

* 事实上排序后选定第二条边，其后的边比第二条边大，必然满足$b - a < c$，无需查找作左区间，因为如果存在，必然是第二条边之后的边。这样只需查找右区间。注意到随着遍历，右区间只会增，这样可以在前一步的右区间基础上继续遍历。

```java
class Solution {

    public int triangleNumber(int[] nums) {
        if (nums == null || nums.length < 3) return 0;

        int count = 0;
        Arrays.sort(nums);

        for (int i = 0; i < nums.length - 2; i++) {
            if (nums[i] <= 0) continue;

            int k = i + 2;
            for (int j = i + 1; j < nums.length - 1; j++) {
                // 在前一轮k的基础上继续遍历
                while (k < nums.length && nums[k] < nums[i] + nums[j]) k++;
                count += k - j - 1;
            }
        }
        return count;
    }
}
```

## 性能分析

&emsp;二分查找的时间复杂度为$O(N^2\log_2N)$，空间复杂度为$O(1)$。

执行用时：170ms，在所有java提交中击败了37.41%的用户。

内存消耗：39.6MB，在所有java提交中击败了66.67%的用户。

&emsp;思路优化后时间复杂度为$O(N^2)$，空间复杂度为$O(1)$。

执行用时：12ms，在所有java提交中击败了73.44%的用户。

内存消耗：39.3MB，在所有java提交中击败了66.67%的用户。

## 官方解题

&emsp;同上。