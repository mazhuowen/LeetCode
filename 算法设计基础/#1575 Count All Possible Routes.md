[toc]

You are given an array of **distinct** positive integers locations where `locations[i]` represents the position of city `i`. You are also given integers `start`, `finish` and `fuel` representing the starting city, ending city, and the initial amount of fuel you have, respectively.

At each step, if you are at city `i`, you can pick any city `j` such that $j \ne i$ and $0 \le j < \text{locations.length}$ and move to city `j`. Moving from city `i` to city `j` reduces the amount of fuel you have by `|locations[i] - locations[j]|`. Please notice that `|x|` denotes the absolute value of `x`.

Notice that `fuel` **cannot** become negative at any point in time, and that you are **allowed** to visit any city more than once (including `start` and `finish`).

Return the count of all possible routes from `start` to `finish`.

Since the answer may be too large, return it modulo $10^9 + 7$.



**Constraints**:

* $2 \le \text{locations.length} \le 100$
* $1 \le \text{locations[i]} \le 10^9$
* All integers in `locations` are **distinct**.
* $0 \le \text{start, finish} < \text{locations.length}$
* $1 \le \text{fuel} \le 200$



## 题目解读

&emsp;求从起始城市到目标城市的路径数目，城市可重复经过。

```java
class Solution {
    public int countRoutes(int[] locations, int start, int finish, int fuel) {

    }
}
```

## 程序设计

* 首先想到的是记忆化回溯，数组`dp(i,j)`表示在城市$i$时有$j$的油量的情况下到达终点的路径数目；

```java
class Solution {
    private static final int MOD = 1_000_000_007;
    int[][] dp;
    
    public int countRoutes(int[] locations, int start, int finish, int fuel) {
        if (fuel < 0) return 0;
        int n = locations.length;
        // 初始化
        dp = new int[n][fuel + 1];
        for (int[] array : dp) Arrays.fill(array, -1);
        
        // 回溯
        return backTracing(start, finish, locations, fuel);
    }
    
    private int backTracing(int cur, int finish, int[] locations, int fuel) {
        if (dp[cur][fuel] != -1) return dp[cur][fuel];
        
        // 如果当前是终点，则数目为1（由于到达终点后可继续前行，故到达终点不返回）
        int res = cur == finish ? 1 : 0;
        for (int next = 0; next < locations.length; next++) {
            // 所消耗的油
            int cost = Math.abs(locations[cur] - locations[next]);
            if (next == cur || fuel < cost) continue;
            
            res  = (res + backTracing(next, finish, locations, fuel - cost)) % MOD;
        }
        return dp[cur][fuel] = res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(NM)$。

执行用时：72ms，在所有java提交中击败了88.37%的用户。

内存消耗：39.1MB，在所有java提交中击败了98.67%的用户。

## 官方解题

&emsp;暂无，密切关注。社区有采用动态规划的做法，`dp(i,j)`表示在城市`j`油量为`i`的路径数。

```java
class Solution {
    private static final int MOD = 1_000_000_007;
    
    public int countRoutes(int[] locations, int start, int finish, int fuel) {
        if (fuel < 0) return 0;
        int n = locations.length;
        // 动态规划数组，表示油量为i时在城市j到达目的地的路径数
        int[][] dp = new int[fuel + 1][n];
        
        // 油量从低到高
        for (int i = 0; i <= fuel; i++) {
            // 遍历城市
            for (int j = 0; j < n; j++) {
                // 如果是终点城市，路径数为1（此处不返回）
                if (j == finish) dp[i][j] = 1;
				// 下一个城市
                for (int k = 0; k < n; k++) {
                    int cost = Math.abs(locations[j] - locations[k]);
                    if (j == k || cost > i) continue;
                    dp[i][j] = (dp[i][j] + dp[i - cost][k]) % MOD;
                }
            }
        }
        return dp[fuel][start];
    }
}
```

&emsp;时间复杂度为$O(N^2M)$，空间复杂度为$O(NM)$。

执行用时：229ms，在所有java提交中击败了32.56%的用户。

内存消耗：39.2MB，在所有java提交中击败了97.67%的用户。