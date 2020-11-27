[toc]

Given two arrays of length `m` and `n` with digits `0-9` representing two numbers. Create the maximum number of length $k \le m + n$ from digits of the two. The relative order of the digits from the same array must be preserved. Return an array of the `k` digits.



**Note**: You should try to optimize your time and space complexity.



## 题目解读

&emsp;给定两个数组，从中选择出`k`个数字，使得在所有的选择中最大，需注意这`k`个数的组合需要保持数组中的相对位置。

```java
class Solution {
    public int[] maxNumber(int[] nums1, int[] nums2, int k) {

    }
}
```

## 程序设计

* 参考社区，利用贪婪思路分别从两个数组中选出最大的`i`和`k-i`个元素，然后拼接比较。

```java
class Solution {
    public int[] maxNumber(int[] nums1, int[] nums2, int k) {
        if (nums1 == null || nums2 == null || nums1.length + nums2.length < k) throw new IllegalArgumentException("invalid param");

        int[] res = new int[k];
        int m = nums1.length, n = nums2.length;
        // 尝试所有组合
        for (int i = Math.max(0, k - n); i <= k && i <= m; i++) {
            int[] arr = merge(maxArr(nums1, i), maxArr(nums2, k - i));
            // 比较大小
            if (gt(arr, res, 0, 0)) res = arr;
        }
        return res;
    }

    // 查找数组中最大的k个元素序列
    private int[] maxArr(int[] nums, int k) {
        int[] res = new int[k];
        // idx为下一次放入值的位置
        int n = nums.length, idx = 0;
        for (int i = 0; i < n; i++) {
            // 数组中可选的元素多于答案剩余所需元素，则贪婪将当前元素放到其能达到的最前面
            while (idx > 0 && n - i - 1 >= k - idx && nums[i] > res[idx - 1]) idx--;
            // 赋值
            if (idx < k) res[idx++] = nums[i];
        }
        return res;
    }

    // 拼接归并
    private int[] merge(int[] nums1, int[] nums2) {
        int m = nums1.length, n = nums2.length;
        int[] res = new int[m + n];
        int idx = 0, idx1 = 0, idx2 = 0;
        // 注意不是选择当前值最大的，而是选择和后面值结合最大的当前元素
        while (idx < m + n) {
            res[idx++] = gt(nums1, nums2, idx1, idx2) ? nums1[idx1++] : nums2[idx2++];
        }
        return res;
    }

    private boolean gt(int[] nums1, int[] nums2, int i, int j) {
        int m = nums1.length, n = nums2.length;
        while (i < m && j < n && nums1[i] == nums2[j]) {
            i++;
            j++;
        }
        //  第二个数组先遍历完，或第一个数组值比第二个数组大
        return j == n || (i != m && nums1[i] > nums2[j]);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(K\max(M,N))$，空间复杂度为$O(K)$。

执行用时：8ms，在所有java提交中击败了98.47%的用户。

内存消耗：40.5MB，在所有java提交中击败了8.70%的用户。

## 官方解题

&emsp;暂无，密切关注。