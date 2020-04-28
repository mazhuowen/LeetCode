[toc]

Given a list of **non-negative** numbers and a target **integer** k, write a function to check if the array has a continuous subarray of size at least 2 that sums up to a multiple of **k**, that is, sums up to $n*k$ where n is also an **integer**.



Note:

* The length of the array won't exceed $10000$.
* You may assume the sum of all the numbers is in the range of a signed 32-bit integer.



## 题目解读

&emsp;判断数组中长度大于2的子数组之和是否是$k$的倍数。

```java
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {

    }
}
```

## 程序设计

* 使用前缀和记录数组，遍历判断。需注意$k = 0$的情况。

```java
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {
        if (nums == null || nums.length == 0) return false;

        int[] preSum = new int[nums.length + 1];
        for (int i = 0; i < nums.length; i++) preSum[i + 1] = preSum[i] + nums[i];

        for (int i = 0; i < preSum.length; i++) {
            for (int j = i + 2; j < preSum.length; j++) {
                int num = preSum[j] - preSum[i];
                if (k != 0 && num % k == 0) return true;
                if (k == 0 && num == 0) return true;
            }
        }
        return false;
    }
}
```

* 优化逻辑，将$k = 0$的判断提前到第一个循环中来。

```java
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {
        if (nums == null || nums.length == 0) return false;

		// 前缀和
        int[] preSum = new int[nums.length + 1];
        for (int i = 0; i < nums.length; i++) preSum[i + 1] = preSum[i] + nums[i];

        for (int i = 0; i < preSum.length; i++) {
            // k为0只需判断其后第二个前缀和是否相等
            if (k == 0) {
                if (i + 2 < preSum.length && preSum[i] == preSum[i + 2]) return true;
                else continue;
            }
            // 遍历判断是否是k的倍数
            for (int j = i + 2; j < preSum.length; j++) {
                int num = preSum[j] - preSum[i];
                if (k != 0 && num % k == 0) return true;
            }
        }
        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：19ms，在所有java提交中击败了51.13%的用户。

内存消耗：40.8MB，在所有java提交中击败了20.00%的用户。

## 官方解题

&emsp;除了上述思路，官方还提供了哈希表记录前缀和的思路。单纯记录前缀和仍然需要遍历计算，考虑到$sum2 - sum1 = k * n \implies sum2\%k = sum1\%k$，故可以保存前缀和取余后的数值。对于特殊情况$k = 0$则不做取余操作。

```java
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {
        if (nums == null || nums.length == 0) return false;

        // 记录前缀和和索引
        Map<Integer, Integer> counter = new HashMap<>();
        counter.put(0, -1);

        int preSum = 0;
        for (int i = 0; i < nums.length; i++) {
            preSum += nums[i];

            if (k != 0) preSum %= k;
			// 存在，则判断距离是否符合要求
            if (counter.containsKey(preSum)) {
                if (i - counter.get(preSum) > 1) return true;
            } 
            // 加入
            else {
                counter.put(preSum, i);
            }
        }
        return false;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了99.43%的用户。

内存消耗：40.3MB，在所有java提交中击败了20.00%的用户。