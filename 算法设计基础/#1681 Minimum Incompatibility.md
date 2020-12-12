[toc]

You are given an integer array `nums` and an integer $k$. You are asked to distribute this array into $k$ subsets of **equal size** such that there are no two equal elements in the same subset.

A subset's **incompatibility** is the difference between the maximum and minimum elements in that array.

Return the **minimum possible sum of incompatibilities** of the $k$ subsets after distributing the array optimally, or return $-1$ if it is not possible.

A subset is a group integers that appear in the array with no particular order.

 

**Example 1**:

```
Input: nums = [1,2,1,4], k = 2
Output: 4
Explanation: The optimal distribution of subsets is [1,2] and [1,4].
The incompatibility is (2-1) + (4-1) = 4.
Note that [1,1] and [2,4] would result in a smaller sum, but the first subset contains 2 equal elements.
```

**Example 2**:

```
Input: nums = [6,3,8,1,3,1,2,2], k = 4
Output: 6
Explanation: The optimal distribution of subsets is [1,2], [2,3], [6,8], and [1,3].
The incompatibility is (2-1) + (3-2) + (8-6) + (3-1) = 6.
```

**Example 3**:

```
Input: nums = [5,3,3,6,3,3], k = 3
Output: -1
Explanation: It is impossible to distribute nums into 3 subsets where no two elements are equal in the same subset.
```



**Constraints**:

* $1 \le k \le \text{nums.length} \le 16$
* `nums.length` is divisible by $k$
* $1 \le \text{nums[i]} \le \text{nums.length}$



## 题目解读

&emsp;将数组分成$k$个子集，且每个子集数字不相等，求每个子集最大值最小资之差的最小和。

```java
class Solution {
    public int minimumIncompatibility(int[] nums, int k) {

    }
}
```

## 程序设计

* 参考社区思路，采用`dp(i,j)`表示当前状态为$j$，前一次选择的值索引为$i$的最小差值之和；
* 首先排序处理数组，处理$k = 1$及无法分配的特殊情况；这样做的好处是可以在后续回溯时避免重复尝试；由于需要尝试分配$k$个子集，如果没有排重机制，会导致非法或非最优分配在$k=1$时尝试后，在其余$k$也尝试，可通过每次起始给当前$k$分配最小值，使得每个$k$尝试集合唯一不重复；
* 分配有两种情况，一种是分配完一个$k$，分配下一个，此时如上述分析，选择未分配值中最小的数，同时由于此时状态与前一时刻选择的值无关，对每个`dp(i,stat)`赋值；另一种情况是还在分配$k$中，则尝试分配下一个数字并计算。

```java
class Solution {
    private int baseLen;
    private int[][] record;

    public int minimumIncompatibility(int[] nums, int k) {
        int n = nums.length;
        if (n % k != 0) return -1;
        if (k == n) return 0;
        baseLen = n / k;
        // 排序数组
        Arrays.sort(nums);
        // 判断是否分为k组后有重复数字
        int pre = -1, count = 0;
        for (int num : nums) {
            if (num != pre) {
                pre = num;
                count = 0;
            }
            // 存在重复数字
            if (++count > k) return -1;
        }
        if (k == 1) return nums[n - 1] - nums[0];

        // 记忆化数组，表示当前选择i，状态为j的差值和
        record = new int[n][1 << n];
        for (int[] arr : record) Arrays.fill(arr, -1);
        return backTracing((1 << n) - 1, 0, n, nums);
    }

    // 表示当前状态、上次选择值索引、可用数目、数组
    private int backTracing(int stat, int pre, int count, int[] nums) {
        // 全部分配完，返回
        if (stat == 0) return 0;
        if (record[pre][stat] != -1) return record[pre][stat];

        // 避免溢出
        int res = Integer.MAX_VALUE / 2;
        // 新分配子集
        if (count % baseLen == 0) {
            // 选择未分配的值中最小的数字开始分配（索引最小，即stat最左端的第一个1对应的索引）
            res = backTracing(stat ^ (stat & -stat), Integer.numberOfTrailingZeros(stat), count - 1, nums);
            for (int i = 0; i < record.length; i++) {
                record[i][stat] = res;
            }
        } 
        // 在原先子集继续分配
        else {
            // 由于是从小到大分配，故检测剩余高位（pre及之前已分配）是否还有足够的分配数目
            if (Integer.bitCount(stat >> (pre + 1)) >= count % baseLen) {
                for (int i = pre + 1; i < record.length; i++) {
                    // 数值不重复且未分配，则尝试
                    if (nums[i] > nums[pre] && (stat & (1 << i)) != 0) res = Math.min(res, backTracing(stat ^ (1 << i), i, count - 1, nums) + nums[i] - nums[pre]);
                }
            }

            record[pre][stat] = res;
        }
        return record[pre][stat];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^22^N)$，空间复杂度为$O(N2^N)$。

执行用时：16 ms, 在所有 Java 提交中击败了93.91%的用户

内存消耗：47.3 MB, 在所有 Java 提交中击败了13.10%的用户

## 官方解题

&emsp;暂无，密切关注。
