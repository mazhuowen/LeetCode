[toc]

Given an array `nums` of $n$ integers where $n > 1$,  return an array output such that `output[i]` is equal to the product of all the elements of `nums` except `nums[i]`.



**Note**: Please solve it without division and in $O(n)$.

**Follow up**:
Could you solve it with constant space complexity? (The output array does not count as extra space for the purpose of space complexity analysis.)



## 题目解读

&emsp;计算除了$i$之外其他位置数字乘积。题目要求不能使用除法，且空间复杂度有限制。

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        
    }
}
```

## 程序设计

* 最基本的做法是前缀积。

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        if (nums == null || nums.length <= 1) return nums;

        int n = nums.length;
        // 前缀积
        int[] left = new int[n];
        int[] right = new int[n];
        left[0] = right[n - 1] = 1;

        for (int i = 1; i < n; i++) {
            left[i] = left[i - 1] * nums[i - 1];
        }
        for (int i = n - 2; i >= 0; i--) {
            right[i] = right[i + 1] * nums[i + 1];
        }

        int[] res = new int[n];
        for (int i = 0; i < n; i++) {
            res[i] = left[i] * right[i];
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：48.2MB，在所有java提交中击败了11.76%的用户。

## 官方解题

&emsp;官方在上述思路基础上持续优化：

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        if (nums == null || nums.length <= 1) return nums;

        int n = nums.length;
        int[] res = new int[n];

        // 计算左前缀积
        res[0] = 1;
        for (int i = 1; i < n; i++) {
            res[i] = res[i - 1] * nums[i - 1];
        }
        // 当前右边前缀积
        int right = 1;
        for (int i = n - 1; i >= 0; i--) {
            res[i] *= right;
            right *= nums[i];
        }
        return res;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：48.2MB，在所有java提交中击败了11.76%的用户。