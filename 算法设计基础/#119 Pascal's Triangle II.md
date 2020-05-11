[toc]

Given a non-negative index k where k ≤ 33, return the kth index row of the Pascal's triangle.

Note that the row index starts from 0.

In Pascal's triangle, each number is the sum of the two numbers directly above it.



**Follow up:**

Could you optimize your algorithm to use only $O(k)$ extra space?



## 题目解读

&emsp;求解杨辉三角指定行的数据。

```java
class Solution {
    public List<Integer> getRow(int rowIndex) {

    }
}
```

## 程序设计

* 动态规划。

```java
class Solution {
    public List<Integer> getRow(int rowIndex) {
        int[] dp = new int[rowIndex + 1];
        // 初始化第一行
        dp[0] = 1;
        for (int i = 1; i <= rowIndex; i++) {
            for (int j = i - 1; j >= 1; j--) {
                dp[j] += dp[j - 1];
            }
            // 末尾添加1
            dp[i] = 1;
        }
        List<Integer> res = new ArrayList<>();
        for (int num : dp) res.add(num);
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(k^2)$，空间复杂度为$O(k)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37MB，在所有java提交中击败了5.88%的用户。

## 官方解题

&emsp;同上。