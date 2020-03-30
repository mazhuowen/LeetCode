[toc]

Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

Note:

Bonus point if you are able to do this using only $O(n)$ extra space, where n is the total number of rows in the triangle.



## 题目解读

&emsp;给定一个三角形，找出自顶向下的最小路径和。每一步只能移动到下一行中相邻的结点上。

```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {

    }
}
```

## 程序设计

* 动态规划问题，需要特殊考虑两条边上的结点递归情况。

```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        if (triangle == null || triangle.size() == 0) return 0;
        int n = triangle.size();

        int[] dp = new int[n];
        dp[0] = triangle.get(0).get(0);
        int min = dp[0];
        for (int i = 1; i < n; i++) {
            min = Integer.MAX_VALUE;
            // 注意反序更新
            for (int j = i; j >= 0; j--) {
                if (j == 0) dp[j] += triangle.get(i).get(j);
                else if (j == i) dp[j] = dp[j - 1] + triangle.get(i).get(j);
                else dp[j] = Math.min(dp[j - 1], dp[j]) + triangle.get(i).get(j);

                min = Math.min(min, dp[j]);
            }
        }
        return min;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了79.64%的用户。

内存消耗：40MB，在所有java提交中击败了5.32%的用户。

## 官方解题

&emsp;暂无，密切关注。