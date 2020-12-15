[toc]

We define a harmonious array as an array where the difference between its maximum value and its minimum value is **exactly** $1$.

Given an integer array `nums`, return the length of its longest harmonious subsequence among all its possible subsequences.

A **subsequence** of array is a sequence that can be derived from the array by deleting some or no elements without changing the order of the remaining elements.

 

**Example 1**:

```
Input: nums = [1,3,2,2,5,2,3,7]
Output: 5
Explanation: The longest harmonious subsequence is [3,2,2,2,3].
```

**Example 2**:

```
Input: nums = [1,2,3,4]
Output: 2
```

**Example 3**:

```
Input: nums = [1,1,1,1]
Output: 0
```



**Constraints**:

* $1 \le \text{nums.length} \le 2 * 10^4$
* $-10^9 \le \text{nums[i]} \le 10^9$



## 题目解读

&emsp;和谐数组定义为最小值、最大值之差为$1$的数组，求给定数组的子序列构成的最长和谐数组。

```java
class Solution {
    public int findLHS(int[] nums) {

    }
}
```

## 程序设计

* 采用哈希表存储数字计数。

```java
class Solution {
    public int findLHS(int[] nums) {
        int max = 0;
        Map<Integer, Integer> counter = new HashMap<>();
        for (int num : nums) {
            int cur = counter.getOrDefault(num, 0) + 1;
            counter.put(num, cur);
            if (counter.containsKey(num - 1)) max = Math.max(max, cur + counter.get(num - 1));
            if (counter.containsKey(num + 1)) max = Math.max(max, cur + counter.get(num + 1));
        }
        return max;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：24 ms, 在所有 Java 提交中击败了45.41%的用户。

内存消耗：39.3 MB, 在所有 Java 提交中击败了88.06%的用户。

## 官方解题

&emsp;上述思路参考官方。社区还有排序后统计相邻数字数目的方法。
