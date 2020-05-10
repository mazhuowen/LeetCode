[toc]

Given a sorted array `nums`, remove the duplicates **in-place** such that duplicates appeared at most twice and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array** **in-place** with $O(1)$ extra memory.



## 题目解读

&emsp;删除有序数组中出现超过两次的数字。

```java
class Solution {
    public int removeDuplicates(int[] nums) {

    }
}
```

## 程序设计

* 数组的移动复制。

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        if (nums == null) throw new IllegalArgumentException("invalid param");

        int idx = 0, count = 1;
        for (int i = 1; i < nums.length; i++) {
            // 超出数目，抛弃
            if (nums[i] == nums[idx] && ++count > 2) continue;

            // 新的数字，重置计数
            if (nums[i] != nums[idx]) {
                count = 1;
            }
            // 赋值，更新索引
            nums[++idx] = nums[i];
        }
        // 长度为索引加一
        return ++idx;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了98.50%的用户。

内存消耗：39.8MB，在所有java提交中击败了8.33%的用户。

## 官方解题

&emsp;官方思路一致，覆盖重复值。实现略繁琐。