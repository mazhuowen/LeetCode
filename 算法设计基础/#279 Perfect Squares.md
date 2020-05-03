[toc]

Given a positive integer n, find the least number of perfect square numbers (for example, $1, 4, 9, 16, \dots$) which sum to $n$.



## 题目解读

&emsp;计算给定数字可以使用平方数之和组成的最少数目。

```java
class Solution {
    public int numSquares(int n) {
        
    }
}
```

## 程序设计

* 使用动态规划数字，`dp(i)`表示$i$可由平方数构成的最少数目。`dp(i) = min(dp(i - j) + dp(j))`。

```java
class Solution {
    public int numSquares(int n) {
        if (n <= 0) throw new IllegalArgumentException("invalid param");

        int[] dp = new int[n + 1];
        // 初始化
        for (int i = 1; i * i <= n; i++) {
            dp[i * i] = 1;
        }
        
        for (int i = 1; i <= n; i++) {
            // 遍历更新
            for (int j = i / 2; j >= 0; j--) {
                if (dp[j] == 0 || dp[i - j] == 0) continue;
                
                if (dp[i] == 0) dp[i] = dp[j] + dp[i - j];
                else dp[i] = Math.min(dp[i], dp[j] + dp[i - j]);
            }
        }
        return dp[n];
    }
}
```

* 上述遍历每一个值，实际上只需遍历`j`为平方数的结果。

```java
class Solution {
    public int numSquares(int n) {
        if (n <= 0) throw new IllegalArgumentException("invalid param");

        int[] dp = new int[n + 1];

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j * j <= i; j++) {
                if (dp[i] == 0) dp[i] = dp[i - j * j] + 1;
                else dp[i] = Math.min(dp[i], dp[i - j * j] + 1);
            }
        }
        return dp[n];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时 :1317 ms, 在所有 Java 提交中击败了5.00%的用户。

内存消耗 :39.5 MB, 在所有 Java 提交中击败了10.53%的用户。

&emsp;时间复杂度为$O(N\sqrt{N})$，空间复杂度为$O(N)$。

执行用时：42ms，在所有java提交中击败了59.94%的用户。

内存消耗：39.2MB，在所有java提交中击败了10.53%的用户。

## 官方解题

&emsp;动态规划求解了所有的平方和，实际大部分是不需要的。官方提供了贪心枚举的思路。

```java
class Solution {
    Set<Integer> set;

    public int numSquares(int n) {
        // 维护平方数
        set = new HashSet<>();
        for (int i = 1; i * i <= n; i++) {
            set.add(i * i);
        }
		// 数目从低到高遍历判断
        for (int i = 1; i <= n; i++) {
            if (isDivide(n, i)) return i;
        }
        return n;
    }

    private boolean isDivide(int num, int count) {
        // 递归终止条件
        if (count == 1) return set.contains(num);

        for (int square : set) {
            if (isDivide(num - square, count - 1)) return true;
        }
        return false;
    }
}
```

&emsp;时间复杂度为$O(N^{\frac{h}{2}})$，空间复杂度为$O(\sqrt{N})$。

执行用时：14ms，在所有java提交中击败了93.42%的用户。

内存消耗：39.6MB，在所有java提交中击败了10.53%的用户。