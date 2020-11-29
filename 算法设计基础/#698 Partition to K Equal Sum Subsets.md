[toc]

Given an array of integers `nums` and a positive integer $k$, find whether it's possible to divide this array into $k$ non-empty subsets whose sums are all equal.



**Note:**

- $1 \le k \le \text{len(nums)} \le 16$.
- $0 < \text{nums[i]} < 10000$.



## 题目解读

&emsp;判断数组是否可分成元素和相等的$k$部分。

```java
class Solution {
    public boolean canPartitionKSubsets(int[] nums, int k) {

    }
}
```

## 程序设计

* 参考官方解题，采用将当前数字尝试分到$k$个桶中的一个的思路，进行回溯。时间复杂度为$O(K^{N - K}K!)$，会超时。

```java
class Solution {
    public boolean canPartitionKSubsets(int[] nums, int k) {
        if (nums == null || nums.length == 0) return false;

        // 检查和是否可分
        int sum = 0;
        for (int num : nums) sum += num;
        if (sum % k != 0) return false;

        sum /= k;

        return canPartitionKSubsets(new int[k], nums, 0, sum);
    }

    private boolean canPartitionKSubsets(int[] bucket, int[] nums, int start, int target) {
        // 所有数字分配完，有效
        if (start >= nums.length) return true;
        int num = nums[start++];
        // 将start位置的数分配到k个桶中
        for (int i = 0; i < bucket.length; i++) {
            if (bucket[i] + num > target) continue;
            bucket[i] += num;
            if (canPartitionKSubsets(bucket, nums, start, target)) return true;
            bucket[i] -= num;
        }
        return false;
    }
}
```

* 优化上述代码，在回溯前先进行排序和判断，缩小$k$的范围。

```java
class Solution {
    public boolean canPartitionKSubsets(int[] nums, int k) {
        if (nums == null || nums.length == 0) return false;

        // 检查和是否可分
        int sum = 0;
        for (int num : nums) sum += num;
        if (sum % k != 0) return false;

        sum /= k;

        // 排序判断
        Arrays.sort(nums);
        int start = nums.length - 1;
        // 判断缩小回溯范围
        if (nums[start] > sum) return false;
        while (start >= 0 && nums[start] == sum) {
            start--;
            k--;
        }

        return canPartitionKSubsets(new int[k], nums, start, sum);
    }

    private boolean canPartitionKSubsets(int[] bucket, int[] nums, int start, int target) {
        // 所有数字分配完，有效
        if (start < 0) return true;
        int num = nums[start--];
        // 将start位置的数分配到k个桶中
        for (int i = 0; i < bucket.length; i++) {
            if (bucket[i] + num > target) continue;
            bucket[i] += num;
            if (canPartitionKSubsets(bucket, nums, start, target)) return true;
            bucket[i] -= num;
        }
        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(K^{N - K}K!)$，凯南复杂度为$O(N)$。

执行用时：27ms，在所有java提交中击败了32.71%的用户。

内存消耗：37.5MB，在所有java提交中击败了9.09%的用户。

## 官方解题

&emsp;官方解题一个重要优化是加入判断桶空的逻辑，由于有$k$个桶，如果不加判断，回溯会有很多重复值，比如同样的组合在第一个桶中不符合要求，放到第二个桶仍任不符合要求；如果第一个桶回溯到$0$，说明不符合要求，后续桶就不要再回溯，等到第一个桶尝试成功，再尝试后续的桶。这样大大减少尝试次数。

```java
class Solution {
    public boolean canPartitionKSubsets(int[] nums, int k) {
        if (nums == null || nums.length == 0) return false;

        // 检查和是否可分
        int sum = 0;
        for (int num : nums) sum += num;
        if (sum % k != 0) return false;

        sum /= k;

        // 排序判断
        Arrays.sort(nums);
        int start = nums.length - 1;
        if (nums[start] > sum) return false;
        while (start >= 0 && nums[start] == sum) {
            start--;
            k--;
        }

        return canPartitionKSubsets(new int[k], nums, start, sum);
    }

    private boolean canPartitionKSubsets(int[] bucket, int[] nums, int start, int target) {
        // 所有数字分配完，有效
        if (start < 0) return true;
        int num = nums[start--];
        // 将start位置的数分配到k个桶中
        for (int i = 0; i < bucket.length; i++) {
            if (bucket[i] + num > target) continue;
            bucket[i] += num;
            if (canPartitionKSubsets(bucket, nums, start, target)) return true;
            bucket[i] -= num;
            // 重要剪枝，只有当前一个桶尝试成功才尝试下一个桶
            if (bucket[i] == 0) break;
        }
        return false;
    }
}
```

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.4MB，在所有java提交中击败了9.09%的用户。