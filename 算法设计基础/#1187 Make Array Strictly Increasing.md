[toc]

Given two integer arrays `arr1` and `arr2`, return the minimum number of operations (possibly zero) needed to make `arr1` strictly increasing.

In one operation, you can choose two indices $0 \le i < \text{arr1.length}$ and $0 \le j < \text{arr2.length}$ and do the assignment $\text{arr1[i]} = \text{arr2[j]}$.

If there is no way to make `arr1` strictly increasing, return `-1`.



Constraints:

* $1 \le \text{arr1.length, arr2.length} \le 2000$
* $0 \le \text{arr1[i], arr2[i]} \le 10^9$



## 题目解读

&emsp;从数组2中选取数值替换数组1中的数字，使得数组1严格递增，求最小替换数。

```java
class Solution {
    public int makeArrayIncreasing(int[] arr1, int[] arr2) {

    }
}
```

## 程序设计

* 令动态规划数组`dp(i,j)`表示`0~i`序列替换`j`次后形成的递增序列最小结尾值，不存在则为无穷大。对于当前位置`i`，有两种选择：

  * 如果当前值`val`比上一个序列的末尾值大，则可直接拼接形成递增序列，无需替换，即`if(do(i - 1,j) < val) dp(i,j) = val`；
  * 如果数组2中存在大于`dp(i-1,j-1)`的值`val`，可替换数组1中的值，从而`dp(i,j) = val`；

  每次更新在上述结果中选择结尾最小的值。

```java
class Solution {
    public int makeArrayIncreasing(int[] arr1, int[] arr2) {
        if (arr1 == null || arr2 == null) throw new IllegalArgumentException("invalid param");

        Arrays.sort(arr2);
        int m = arr1.length, n = arr2.length;
        // dp(i,j)表示前i个序列替换j次后得到的严格递增序列末尾最小值，不存在则为极值
        int[][] dp = new int[m + 1][m + 1];
        // 初始化为极值
        for (int[] nums : dp) Arrays.fill(nums, Integer.MAX_VALUE);
        // 初始化起始为最小
        dp[0][0] = Integer.MIN_VALUE;

        // 序列
        for (int i = 1; i <= m; i++) {
            // 替换数，不能超过序列数
            for (int j = 0; j <= i; j++) {
                // 如果当前值比前一递增序列末尾值大，可直接拼接
                if (dp[i - 1][j] < arr1[i - 1]) dp[i][j] = arr1[i - 1];

                // 如果存在比末尾小的值，替换形成递增序列
                int replace;
                if (j > 0 && (replace = binarySearch(arr2, dp[i - 1][j - 1])) != -1) {
                    // 选择较小值
                    dp[i][j] = Math.min(dp[i][j], arr2[replace]);
                }

                // 由于替换数j是从小到大遍历，如果当前已经有值，说明是最小操作数，返回
                if (i == m && dp[i][j] < Integer.MAX_VALUE) return j;
            }
        }
        // 不存在
        return -1;
    }

    // 二分查找大于目标值的最小值
    private int binarySearch(int[] arr2, int target) {
        int left = 0, right = arr2.length;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (arr2[mid] > target) right = mid;
            else left = mid + 1;
        }
        return left == arr2.length ? -1 : left;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2\log_2M)$，空间复杂度为$O(N^2)$。

执行用时：214ms，在所有java提交中击败了48.08%的用户。

内存消耗：52.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。