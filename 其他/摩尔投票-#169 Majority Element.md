[toc]

Given an array of size n, find the majority element. The majority element is the element that appears **more than** `[ n/2 ]` times.

You may assume that the array is non-empty and the majority element always exist in the array.



## 题目解读

&emsp;找到数组中占大多数的元素。题目限定数组中存在超过数组一半的元素。

```java
class Solution {
    public int majorityElement(int[] nums) {

    }
}
```

## 程序设计

* 最简单的思路是排序，然后返回中间数，就是众数。

```java
class Solution {
    public int majorityElement(int[] nums) {
        Arrays.sort(nums);
        return nums[nums.length / 2];
    }
}
```

* 实际上不需要全部排序，可以借鉴快速排序的划分，众数的划分必然超过一半，根据这个原理找到众数。

```java
class Solution {
    public int majorityElement(int[] nums) {
        return quickSort(nums, 0, nums.length - 1, nums.length);
    }

    private int quickSort(int[] nums, int start, int end, int len) {
        int left = start, right = end;
        // 以左边点为划分基准点
        int base = nums[left];
        while (left < right) {
            while (left < right && nums[right] >= base) right--;
            if (left < right) nums[left++] = nums[right];

            while (left < right && nums[left] <= base) left++;
            if (left < right) nums[right--] = nums[left];
        }
        nums[left] = base;

        // 如果划分点就在中点，返回
        if (left == len / 2) return base;
        // 缩小范围继续划分
        if (left < len / 2) {
            return quickSort(nums, left + 1, end, len);
        } else {
            return quickSort(nums, start, left - 1, len);
        }
    }
}
```

## 性能分析

&emsp;排序时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了84.22%的用户。

内存消耗：42.7MB，在所有java提交中击败了21.97%的用户。

&emsp;利用快速排序分治思想的改造算法，时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：456ms，在所有java提交中击败了5.18%的用户。

内存消耗：44.5MB，在所有java提交中击败了5.17%的用户。

## 官方解题

&emsp;官方提出了分治法，即统计左右区间的众数，然后再统计当前区间。

```java
class Solution {
    public int majorityElement(int[] nums) {
        return majorityElementRec(nums, 0, nums.length-1);
    }

    private int majorityElementRec(int[] nums, int lo, int hi) {
        if (lo == hi) {
            return nums[lo];
        }

        int mid = (hi-lo)/2 + lo;
        int left = majorityElementRec(nums, lo, mid);
        int right = majorityElementRec(nums, mid+1, hi);

        if (left == right) return left;

        int leftCount = countInRange(nums, left, lo, hi);
        int rightCount = countInRange(nums, right, lo, hi);

        return leftCount > rightCount ? left : right;
    }

    private int countInRange(int[] nums, int num, int lo, int hi) {
        int count = 0;
        for (int i = lo; i <= hi; i++) {
            if (nums[i] == num) {
                count++;
            }
        }
        return count;
    }
}
```

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：3ms，在所有java提交中击败了53.80%的用户。

内存消耗：42.7MB，在所有java提交中击败了18.23%的用户。