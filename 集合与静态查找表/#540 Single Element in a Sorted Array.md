[toc]

You are given a sorted array consisting of only integers where every element appears exactly twice, except for one element which appears exactly once. Find this single element that appears only once.

 

**Note:** Your solution should run in $O(\log_2n)$ time and $O(1)$ space.



## 题目解读

&emsp;排序数组中除了一个数是单独的，其它数都是成对的，查找这个数。

```java
class Solution {
    public int singleNonDuplicate(int[] nums) {

    }
}
```

## 程序设计

* 设单独数字为`x`，则在其前面的数字，第一个数字索引为偶数，在其后的第一个数字索引为奇数，根据这个规律可以进行二分查找。
* 需注意在缩小范围时需要判断移动两位还是一位。

```java
class Solution {
    public int singleNonDuplicate(int[] nums) {
        if (nums == null || nums.length == 0) throw new IllegalArgumentException("invalid param");

        // nums.length为标识位置，因为最终right停留在不重复数字的后面
        // 如果不重复数字就在最后，则会造成误判
        int left = 0, right = nums.length;
        while (left < right) {
            int mid = left + (right - left) / 2;
            // 定位到相同数字的第一个数字
            if (mid - 1 >= 0 && nums[mid] == nums[mid - 1]) mid--;

            // 第一个数字索引为奇数，表示不重复数在前面
            if (mid % 2 == 1) {
                right = mid;
            }
            // 为偶数，在后面
            else {
                // 由于mid可能减一操作，需要判断，left定位到下一个数字，否则会造成死循环
                left = mid + (mid + 1 < nums.length && nums[mid] == nums[mid + 1] ? 2 : 1);
            }
        }
        return nums[left - 1];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.7MB，在所有java提交中击败了20.00%的用户。

## 官方解题

&emsp;官方思路类似，实现更为简洁。

```java
class Solution {
    public int singleNonDuplicate(int[] nums) {
        int lo = 0;
        int hi = nums.length - 1;
        while (lo < hi) {
            int mid = lo + (hi - lo) / 2;
            if (mid % 2 == 1) mid--;
            if (nums[mid] == nums[mid + 1]) {
                lo = mid + 2;
            } else {
                hi = mid;
            }
        }
        return nums[lo];
    }
}
```