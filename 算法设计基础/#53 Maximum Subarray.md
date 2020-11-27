[toc]

Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.



**Follow up**:

If you have figured out the $O(n)$ solution, try coding another solution using the divide and conquer approach, which is more subtle.



## 题目解读

&emsp;数组连续子序列之和问题。

```java
class Solution {
    public int maxSubArray(int[] nums) {
        
    }
}
```

## 程序设计

* 数组连续子序列之和问题，可使用分治法求解。

```java
class Solution {
    public int maxSubArray(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        return maxSubArray(nums, 0, nums.length - 1);
    }

    private int maxSubArray(int[] num, int start, int end) {
        // 递归终止条件
        if (start == end) return num[start];
        int mid = start + (end - start) / 2;
        int maxLeft = maxSubArray(num, start, mid);
        int maxRight = maxSubArray(num, mid + 1, end);

        // 遍历中间区间最大连续和
        int leftSum = Integer.MIN_VALUE, rightSum = Integer.MIN_VALUE, tempSum = 0;
        for (int i = mid; i >= start ; i--) {
            tempSum += num[i];
            leftSum = Math.max(leftSum, tempSum);
        }
        tempSum = 0;
        for (int i = mid + 1; i <= end; i++) {
            tempSum += num[i];
            rightSum = Math.max(rightSum, tempSum);
        }
        int curMax = leftSum + rightSum;
        return Math.max(curMax, Math.max(maxLeft, maxRight));
    }
}
```

* 时间性能更好的方法可以使用前缀和数组遍历，记录更新。

```java
class Solution {
    public int maxSubArray(int[] nums) {
        if (nums == null || nums.length == 0) return 0;

        // 记录遍历过的最小前缀和，和最大序列和
        int minSum = 0, maxSub = Integer.MIN_VALUE;
        // 前缀数组
        int[] preSum = new int[nums.length + 1];
        for (int i = 0; i < nums.length; i++) {
            preSum[i + 1] = preSum[i] + nums[i];
            // 更新记录
            maxSub = Math.max(maxSub, preSum[i + 1] - minSum);
            minSum = Math.min(minSum, preSum[i + 1]);
        }
        return maxSub;
    }
}
```

## 性能分析

&emsp;分治法时间复杂度为$O(N\log_2N)$，空间复杂度$O(\log_2N)$。

执行用时：1ms，在所有java提交中击败了97.46%的用户。

内存消耗：41.7MB，在所有java提交中击败了7.30%的用户。

&emsp;前缀和的方法时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了97.46%的用户。

内存消耗：41.9MB，在所有java提交中击败了5.94%的用户。

## 官方解题

&emsp;官方还提供了两种解法，首先前缀和的思路可以简化，不需要前缀和数组，简化如下：

```java
class Solution {
    public int maxSubArray(int[] nums) {
        if (nums == null || nums.length == 0) return 0;

        // 记录遍历的序列和，代替数组
        int sum = 0;
        int minSum = 0, maxSub = Integer.MIN_VALUE;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            maxSub = Math.max(maxSub, sum - minSum);
            minSum = Math.min(minSum, sum);
        }
        return maxSub;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了97.46%的用户。

内存消耗：41.9MB，在所有java提交中击败了6.22%的用户。