[toc]

Given an unsorted array of integers, find the length of longest increasing subsequence.



Note:

* There may be more than one LIS combination, it is only necessary for you to return the length.
* Your algorithm should run in $O(n^2)$ complexity.



Follow up: Could you improve it to $O(n\log_2n)$time complexity?



## 题目解读

&emsp;找出最长升序子序列长度。

```java
class Solution {
    public int lengthOfLIS(int[] nums) {

    }
}
```

## 程序设计

* 首先暴力法需要三层循环，时间复杂度为$O(N^3)$，不满足要求。转变思路，如果知道之前连续序列的结尾数字和长度，则可以通过对比，轻易的得到当前数字结尾的最长连续序列，可以使用动态规划实现：

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        if (nums == null || nums.length == 0) return 0;

        int n = nums.length;
        int res = 1;
        // 表示以nums[i]结尾的最长连续序列
        int[] dp = new int[n];
        for (int i = 0; i < n; i++) {
            // 最短为1，初始化
            dp[i] = 1;
            for (int j = 0; j < i; j++) {
                // 能与j组合为新的递增序列
                if (nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            res = Math.max(res, dp[i]);
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：14ms，在所有java提交中击败了57.16%的用户。

内存消耗：37.3MB，在所有java提交中击败了7.14%的用户。

## 官方解题

&emsp;除了动态规划，官方还提供了贪婪法和二分查找的思路。对于同样长度的升序序列，结尾值较小的序列显然更容易和之后的值组成更长的序列。用数组索引$i$表示所有长度为$i + 1$的序列的结尾值，根据贪婪法，如果遍历遇到比这个数组结尾大的值，则可以组成更长的序列，添加该值到后面；如果遇到比结尾值小的数值，则检查长度较小的序列结尾值，选择一个结尾值大的序列替换，这样后续数值更容易组成升序序列。最后返回数组长度。以`0,8,4,12,2`为例，`0`显然放到第一位组成一位的升序序列；`8`比`0`大，组成两位的序列；`4`比`8`小，比`0`大，替换`8`所组成的两位序列结尾值更小；`12`拼接到数组后面；`2`比`0`大，比`4`小，替换`4`组成结尾更小的两位序列。

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        if (nums == null || nums.length == 0) return 0;

        int n = nums.length;
        // 维持递增序列
        int len = 0;
        // 表示序列长度为i+1的升序序列结尾值
        int[] k = new int[n];

        for (int i = 0; i < nums.length; i++) {
            // 比结尾值大，加入结尾
            if (len == 0 || nums[i] > k[len - 1]) {
                k[len++] = nums[i];
            }
            // 插入第一个大于当前值的位置
            else {
                int left = 0, right = len - 1;
                while (left < right) {
                    int mid = left + (right - left) / 2;
                    // right为大于等于num的第一个值
                    if (k[mid] >= nums[i]) {
                        right = mid;
                    } else {
                        left = mid + 1;
                    }
                }
                // 不相等则插入，相等则略过
                if (k[left] != nums[i]) k[left] = nums[i];
            }
        }

        return len;
    }
}
```

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.4MB，在所有java提交中击败了7.14%的用户。