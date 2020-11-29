[toc]

You are given an integer array nums. You can choose **exactly one** index (**0-indexed**) and remove the element. Notice that the index of the elements may change after the removal.

For example, if `nums = [6,1,7,4,1]`:

* Choosing to remove index `1` results in `nums = [6,7,4,1]`.
* Choosing to remove index `2` results in `nums = [6,1,4,1]`.
* Choosing to remove index `4` results in `nums = [6,1,7,4]`.

An array is **fair** if the sum of the odd-indexed values equals the sum of the even-indexed values.

Return the **number** of indices that you could choose such that after the removal, `nums` is **fair**.

 

**Example 1**:

```
Input: nums = [2,1,6,4]
Output: 1
Explanation:
Remove index 0: [1,6,4] -> Even sum: 1 + 4 = 5. Odd sum: 6. Not fair.
Remove index 1: [2,6,4] -> Even sum: 2 + 4 = 6. Odd sum: 6. Fair.
Remove index 2: [2,1,4] -> Even sum: 2 + 4 = 6. Odd sum: 1. Not fair.
Remove index 3: [2,1,6] -> Even sum: 2 + 6 = 8. Odd sum: 1. Not fair.
There is 1 index that you can remove to make nums fair.
```

**Example 2**:

```
Input: nums = [1,1,1]
Output: 3
Explanation: You can remove any index and the remaining array is fair.
```

**Example 3**:

```
Input: nums = [1,2,3]
Output: 0
Explanation: You cannot make a fair array after removing any index.
```



**Constraints**:

* $1 \le \text{nums.length} \le 10^5$
* $1 \le \text{nums[i]} \le 10^4$



## 题目解读

&emsp;给定数组，如果删除某个索引所在数组，使得奇数索引数字和和偶数索引数字和相等，则称为平衡数组；求可删除得到平衡数组的方案数。

```java
class Solution {
    public int waysToMakeFair(int[] nums) {
    
    }
}
```

## 程序设计

* 删除某个数字，其前的奇数索引、偶数索引不变，其后的偶数索引变为奇数索引，奇数索引变为偶数索引；可分别计算出奇数索引前缀和和偶数索引前缀和，假设删除的索引为`i`，则：
  * `i`为奇数，其后`i + 2`等奇数索引变为偶数索引，其后`i + 1`等偶数索引变为奇数索引；
  * `i`为偶数，其后`i + 2`等偶数索引变为奇数索引，其后`i + 1`等奇数索引变为偶数索引；

```java
class Solution {
    public int waysToMakeFair(int[] nums) {
        int n = nums.length;
        // 偶数、奇数索引数字前缀和
        int[] evenSum = new int[(n + 1) / 2 + 1], oddSum = new int[n / 2 + 1];
        for (int i = 0; i < n; i++) {
            if (i % 2 == 0) evenSum[i / 2 + 1] = evenSum[i / 2] + nums[i];
            else oddSum[i / 2 + 1] = oddSum[i / 2] + nums[i];
        }

        int res = 0;
        for (int i = 0; i < n; i++) {
            // 尝试移除当前索引
            if (isFair(i, evenSum, oddSum)) res++;
        }
        return res;
    }

    private boolean isFair(int idx, int[] evenSum, int[] oddSum) {
        // 所在前缀和数组索引
        int evenIdx = (idx + 1) / 2, oddIdx = idx / 2;
        // 移除前的索引不变
        int even = evenSum[evenIdx], odd = oddSum[oddIdx];
        // 移除后的索引奇数变偶数，偶数变奇数
        even += oddSum[oddSum.length - 1] - (idx % 2 == 0 ? oddSum[oddIdx] : oddSum[oddIdx + 1]);
        odd += evenSum[evenSum.length - 1] - (idx % 2 == 0 ? evenSum[evenIdx + 1] : evenSum[evenIdx]);
        return even == odd;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：20ms，在所有java提交中击败了19.81%的用户。

内存消耗：52.4MB，在所有java提交中击败了17.17%的用户。

## 官方解题

&emsp;暂无，密切关注。社区思路类似，但不计算前缀和数组，采用遍历中计算之前的奇数、偶数索引数字和，避免复杂的索引映射转换。

```java
class Solution {
    public int waysToMakeFair(int[] nums) {
        // 偶数、奇数索引前缀和
        int evenSum = 0, oddSum = 0;
        for (int i = 0; i < nums.length; i++) {
            if ((i & 1) == 0) evenSum += nums[i];
            else oddSum += nums[i];
        }

        // 当前遍历的偶数、奇数索引数字和
        int even = 0, odd = 0;
        int res = 0;
        for (int i = 0; i < nums.length; i++) {
            if ((i & 1) == 0) {
                // 删除当前偶数索引
                if (even + oddSum - odd == odd + evenSum - even - nums[i]) res++;
                even += nums[i];
            } else {
                // 删除当前奇数索引
                if (even + oddSum - odd - nums[i] == odd + evenSum - even) res++;
                odd += nums[i];
            }
        }
        return res;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：5ms，在所有java提交中击败了98.95%的用户。

内存消耗：52.1MB，在所有java提交中击败了29.56%的用户。