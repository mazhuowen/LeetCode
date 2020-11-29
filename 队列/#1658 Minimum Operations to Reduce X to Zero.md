[toc]

You are given an integer array `nums` and an integer `x`. In one operation, you can either remove the leftmost or the rightmost element from the array `nums` and subtract its value from `x`. Note that this **modifies** the array for future operations.

Return the **minimum number** of operations to reduce `x` to **exactly** $0$ if it's possible, otherwise, return $-1$.

 

**Example 1**:

```
Input: nums = [1,1,4,2,3], x = 5
Output: 2
Explanation: The optimal solution is to remove the last two elements to reduce x to zero.
```

**Example 2**:

```
Input: nums = [5,6,7,8,9], x = 4
Output: -1
```

**Example 3**:

```
Input: nums = [3,2,20,1,1,3], x = 10
Output: 5
Explanation: The optimal solution is to remove the last three elements and the first two elements (5 operations in total) to reduce x to zero.
```



**Constraints**:

* $1 \le \text{nums.length} \le 10^5$
* $1 \le \text{nums[i]} \le 10^4$
* $1 \le x \le 10^9$



## 题目解读

&emsp;移除数字左右两端的数字，减去这些数字，使得最后结果为$0$，求最少的移动次数。

```java
class Solution {
    public int minOperations(int[] nums, int x) {

    }
}
```

## 程序设计

* 原问题需要考虑最短的前缀数组和后缀数组长度和，情况会比较复杂，但反向考虑原问题转化为寻找最长子数组的问题，可使用哈希表记录之前遍历的前缀和索引即可。

```java
class Solution {
    public int minOperations(int[] nums, int x) {
        int[] preSum = new int[nums.length + 1];
        for (int i = 0; i < nums.length; i++) {
            preSum[i + 1] = preSum[i] + nums[i];
        }
        if (preSum[nums.length] == x) return nums.length;
        else if (preSum[nums.length] < x) return -1;

        int target = preSum[nums.length] - x, min = Integer.MAX_VALUE;
        // 记录前缀和及第一个索引
        Map<Integer, Integer> record = new HashMap<>();
        for (int i = 0; i <= nums.length; i++) {
            // 存在子数组和为target，更新长度
            if (record.containsKey(preSum[i] - target)) {
                min = Math.min(min, nums.length - i + record.get(preSum[i] - target));
            }
            // 第一次出现，记录前缀和索引
            if (!record.containsKey(preSum[i])) record.put(preSum[i], i);
        }
        return min == Integer.MAX_VALUE ? -1 : min;
    }
}
```

* 由于问题中数字都为正数，可以简化为最长窗口的问题。

```java
class Solution {
    public int minOperations(int[] nums, int x) {
        int sum = 0;
        for (int num : nums) sum += num;
        if (sum == x) return nums.length;
        if (sum < x) return -1;
        // 滑动窗口的目标和
        int target = sum - x;

        int maxLen = Integer.MIN_VALUE;
        int left = 0, right = 0, cur = 0;
        while (right < nums.length) {
            cur += nums[right];
            while (cur > target) {
                cur -= nums[left++];
            }
            // 更新最大长度
            if (cur == target) maxLen = Math.max(maxLen, right - left + 1);
            right++;
        }
        return maxLen == Integer.MIN_VALUE ? -1 : nums.length - maxLen;
    }
}
```

## 性能分析

&emsp;前缀和思路时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：74 ms, 在所有 Java 提交中击败了41.71%的用户

内存消耗：55.8 MB, 在所有 Java 提交中击败了35.94%的用户。

&emsp;滑动窗口思路时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：4 ms, 在所有 Java 提交中击败了100.00%的用户。

内存消耗：48.7 MB, 在所有 Java 提交中击败了88.39%的用户。

## 官方解题

&emsp;暂无，密切关注。