[toc]

You may recall that an array `arr` is a **mountain array** if and only if:

* $\text{arr.length} \ge 3$
* There exists some index $i$ (**0-indexed**) with $0 < i < \text{arr.length} - 1$ such that:
  * $\text{arr[0]} < \text{arr[1]} < \dots < \text{arr[i - 1]} < \text{arr[i]}$
  * $\text{arr[i]} > \text{arr[i + 1]} > \dots > \text{arr[arr.length - 1]}$

Given an integer array `nums`, return the **minimum** number of elements to remove to make `nums` a **mountain array**.



**Example 1**:

```
Input: nums = [1,3,1]
Output: 0
Explanation: The array itself is a mountain array so we do not need to remove any elements.
```

**Example 2**:

```
Input: nums = [2,1,1,5,6,2,3,1]
Output: 3
Explanation: One solution is to remove the elements at indices 0, 1, and 5, making the array nums = [1,5,6,3,1].
```

**Example 3**:

```
Input: nums = [4,3,2,1,1,2,3,1]
Output: 4
```

**Example 4**:

```
Input: nums = [1,2,3,4,4,3,2,1]
Output: 1
```



**Constraints**:

* $3 \le \text{nums.length} \le 1000$
* $1 \le \text{nums[i]} \le 10^9$
* It is guaranteed that you can make a mountain array out of `nums`.



## 题目解读

&emsp;将数组元素删除后形成的山顶住宿所需最小删除数。

```java
class Solution {
    public int minimumMountainRemovals(int[] nums) {

    }
}
```

## 程序设计

* 可遍历数组中的每个元素，并当做山顶元素统计最大长度。问题转化为最大递减子序列。

```java
class Solution {
    public int minimumMountainRemovals(int[] nums) {
        if (nums == null || nums.length < 3) throw new IllegalArgumentException("invalid param");

        int maxLen = 0;
        for (int i = 1; i < nums.length - 1; i++) {
            int left = longestDesSeq(nums, i, false);
            int right = longestDesSeq(nums, i, true);
            if (left > 1 && right > 1) maxLen = Math.max(maxLen, left + right - 1);
        }
        return nums.length - maxLen;
    }

    private int longestDesSeq(int[] nums, int start, boolean right) {
        int idx = 0, limit = nums[start];
        if (right) {
            int[] tmp = new int[nums.length - start];
            for (int i = start; i < nums.length; i++) {
                if (nums[i] > limit) continue;

                int replace = binarySearch(tmp, nums[i], idx);
                tmp[replace] = nums[i];
                if (replace == idx) idx++;
            }
        } else {
            int[] tmp = new int[start + 1];
            
            for (int i = start; i >= 0; i--) {
                if (nums[i] > limit) continue;
                int replace = binarySearch(tmp, nums[i], idx);
                tmp[replace] = nums[i];
                if (replace == idx) idx++;
            }
        }
        return idx;
    }

    // 在递减数组中找到第一个大于等于的数
    private int binarySearch(int[] arr, int target, int len) {
        int left = 0, right = len;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (arr[mid] <= target) right = mid;
            else left = mid + 1;
        }
        return left;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2\log_2N)$，空间复杂度为$O(N)$。

执行用时：737 ms, 在所有 Java 提交中击败了5.03%的用户。

内存消耗：38.4 MB, 在所有 Java 提交中击败了79.24%的用户。

## 官方解题

&emsp;暂无，密切关注。社区采用一次计算好最长递增子序列长度，然后比对计算。

```java
class Solution {
    public int minimumMountainRemovals(int[] nums) {
        if (nums == null || nums.length < 3) throw new IllegalArgumentException("invalid param");

        // 记录截止当前位置的最长递增子序列
        int[] after = new int[nums.length], before = new int[nums.length];
        init(nums, after, before);

        int maxLen = 0;
        for (int i = 1; i < nums.length - 1; i++) {
            int left = before[i];
            int right = after[i];
            if (left > 1 && right > 1) maxLen = Math.max(maxLen, left + right - 1);
        }
        return nums.length - maxLen;
    }

    private void init(int[] nums, int[] after, int[] before) {
        int idx1 = 0, idx2 = 0;
        int[] inc = new int[nums.length], dec = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            int replace = binarySearch(inc, nums[i], idx1);
            inc[replace] = nums[i];
            if (replace == idx1) idx1++;
            // 截止i有最长replace个子序列
            before[i] = replace + 1;

            replace = binarySearch(dec, nums[nums.length - i - 1], idx2);
            dec[replace] = nums[nums.length - i - 1];
            if (replace == idx2) idx2++;
            // 截止n-i+1最长replace个子序列
            after[nums.length - i - 1] = replace + 1;
        }
    }

    // 在递减数组中找到第一个大于等于的数
    private int binarySearch(int[] arr, int target, int len) {
        int left = 0, right = len;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (arr[mid] >= target) right = mid;
            else left = mid + 1;
        }
        return left;
    }
}
```

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：13 ms, 在所有 Java 提交中击败了93.08%的用户。

内存消耗：38.5 MB, 在所有 Java 提交中击败了69.81%的用户。