[toc]

You are installing a billboard and want it to have the largest height.  The billboard will have two steel supports, one on each side.  Each steel support must be an equal height.

You have a collection of `rods` which can be welded together.  For example, if you have rods of lengths 1, 2, and 3, you can weld them together to make a support of length 6.

Return the largest possible height of your billboard installation.  If you cannot support the billboard, return 0.



Note:

* $0 \le \text{rods.length} \le 20$
* $1 \le \text{rods[i]} \le 1000$
* The sum of rods is at most `5000`.



## 题目解读

&emsp;广告牌有两个钢制支架，两边各一个，每个钢支架的高度必须相等。给定一堆可以焊接在一起的钢筋，返回广告牌的最大可能安装高度，如果没法安装返回0。

```java
class Solution {
    public int tallestBillboard(int[] rods) {

    }
}
```

## 程序设计

* 最基本的，将问题转换为给数组数字添加符号，每个数字可以添加`+`、`-`或替换为$0$，最后得到的和为$0$，且添加`+`（或`-`）号的数字和最大。回溯形式时间复杂度为$O(3^N)$，会超时。

```java
class Solution {
    int maxLen;

    public int tallestBillboard(int[] rods) {
        if (rods == null) return 0;

        tallestBillboard(rods, 0, 0, 0);
        return maxLen;
    }

    // curLen表示添加+号的数字和，sum表示整个数字和
    private void tallestBillboard(int[] rods, int idx, int curLen, int sum) {
        if (idx >= rods.length) {
            if (sum == 0) maxLen = Math.max(maxLen, curLen);
            return;
        }

        // 试探+、-、0
        tallestBillboard(rods, idx + 1, curLen + rods[idx], sum + rods[idx]);
        tallestBillboard(rods, idx + 1, curLen, sum - rods[idx]);
        tallestBillboard(rods, idx + 1, curLen, sum);
    }
}
```

* 可使用`dp(i,j)`表示前`i`个序列中组成两个支架的差值为`j`的最大支架高度；则遍历到当前钢筋有三个选择：不加入、加入较高支架、加入较低支架：

```java
class Solution {
    public int tallestBillboard(int[] rods) {
        if (rods == null) return 0;

        int n = rods.length;
        int sum = 0;
        for (int rod : rods) sum += rod;

        // 表示前i个钢筋构成的高度差为j的两根支架的最大值
        int[][] dp = new int[n + 1][sum + 1];
        
        // 初始化
        Arrays.fill(dp[0], -1);
        dp[0][0] = 0;

        for (int i = 1; i <= n; i++) {
            // 当前钢筋
            int rod = rods[i - 1];
            for (int j = 0; j <= sum - rod; j++) {
                // 不加入当前钢筋，若比较选择前一序列的值
                if (dp[i][j] != 0) dp[i][j] = Math.max(dp[i][j], dp[i - 1][j]);
                else dp[i][j] = dp[i - 1][j];

                // 不存在差距为j的组合，跳过
                if (dp[i - 1][j] == -1) continue;

                // 加入较高的一端
                dp[i][j + rod] = Math.max(dp[i][j + rod], dp[i - 1][j] + rod);
                // 加入较低的一端，1、较高的一端仍然较高
                if (j - rod >= 0) dp[i][j - rod] = Math.max(dp[i][j - rod], dp[i - 1][j]);
                // 2、原先较低的一端更高
                else dp[i][rod - j] = Math.max(dp[i][rod - j], dp[i - 1][j] - j + rod);
            }
        }
        return dp[n][0];
    }
}
```

* 由于只有前一时刻有关，压缩状态数组得：

```java
class Solution {
    public int tallestBillboard(int[] rods) {
        if (rods == null) return 0;

        int n = rods.length;
        int sum = 0;
        for (int rod : rods) sum += rod;

        // 表示构成的高度差为j的两根支架的最大值
        int[] dp = new int[sum + 1];
        
        // 初始化
        Arrays.fill(dp, -1);
        dp[0] = 0;

        for (int rod : rods) {
            int[] temp = dp.clone();
            for (int j = 0; j <= sum - rod; j++) {
                // 不存在差距为j的组合，跳过
                if (temp[j] == -1) continue;

                // 加入较高的一端
                dp[j + rod] = Math.max(dp[j + rod], temp[j] + rod);
                // 加入较低的一端，1、较高的一端仍然较高
                if (j - rod >= 0) dp[j - rod] = Math.max(dp[j - rod], temp[j]);
                // 2、原先较低的一端更高（较低的一端高度为temp[j] - j，加上当前钢筋长度）
                else dp[rod - j] = Math.max(dp[rod - j], temp[j] - j + rod);
            }
        }
        return dp[0];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NS)$，空间复杂度为$O(NS)$。

执行用时：18ms，在所有java提交中击败了64.71%的用户。

内存消耗：39.6MB，在所有java提交中击败了100.00%的用户。

&emsp;空间优化后，时间复杂度为$O(NS)$，空间复杂度为$O(S)$。

执行用时：10ms，在所有java提交中击败了96.08%的用户。

内存消耗：39.6MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;除了动态规划思路，官方还提供了将数组二分然后使用暴力回溯的方式尝试的思路，时间复杂度为$3^{\frac{N}{2}}$。