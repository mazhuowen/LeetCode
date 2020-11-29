[toc]

You are given an **even** number of people `num_people` that stand around a circle and each person shakes hands with someone else, so that there are `num_people / 2` handshakes total.

Return the number of ways these handshakes could occur such that none of the handshakes cross.

Since this number could be very big, return the answer **mod** $10^9 + 7$



**Constraints:**

- $2 \le \text{num_people} \le 1000$
- $\text{num_people} \% 2 == 0$



## 题目解读

&emsp;找出所有握手不相交的方案数目。

<img src="../images/#1259.png" style="zoom:50%;" />

```java
class Solution {
    public int numberOfWays(int num_people) {

    }
}
```

## 程序设计

* 根据题目可得握手不相交的规律，假设一对握手的人为`A`、`B`，则`A`、`B`之间要么是握手对，要么不存在；不能是`A`、`C`、`B`、`D`，`A`与`B`握手，`C`与`D`握手。有了这个规律可以使用回溯实现，使用布尔数组记录握手的人，握手标记为`true`，由于是从大区间开始，则不相交转化为两个人的握手区间内不存在`true`。该方法会超时。

```java
class Solution {
    int num;

    public int numberOfWays(int num_people) {
        if (num_people <= 0 || num_people % 2 != 0) throw new IllegalArgumentException("invalid param");
        numberOfWays(0, new boolean[num_people], num_people / 2);
        return num;
    }

    private void numberOfWays(int start, boolean[] visited, int count) {
        if (count == 0) {
            num++;
            return;
        }
        // 找到未握手的人
        if (visited[start]) {
            while (visited[start]) start++;
        }
        // 标记为握手
        visited[start] = true;
        // 截止第一个true，避免区间内存在相交情况
        for (int i = start + 1; i < visited.length && !visited[i]; i++) {
            // 试探
            visited[i] = true;
            numberOfWays(start + 1, visited, count - 1);
            // 回溯
            visited[i] = false;
        }
        visited[start] = false;
    }
}
```

* 由于$N$个人站成一个环，以某个人为参照点，假设为图中的$3$，则其可与旁边的$2$或$4$握手，剩余的$N-2$个人仍然围成一个环，有`dp(i) = 2 * dp(i-2)`中方案；除了与旁边划分，$3$还可将人群划分为两块，如图所示，可与$6$、$8$、$10$握手，剩余两群人的握手方案数乘积即可；

<img src="..\images\#1259.jpg" style="zoom: 50%;" />

```java
class Solution {
    private final int MOD = 1_000_000_007;

    public int numberOfWays(int num_people) {
        // 表示i个人的不相交握手方式
        long[] dp = new long[num_people + 1];
        // 初始化（0个人、奇数个人握手方案数为0）
        dp[2] = 1;
        for (int i = 4; i <= num_people; i += 2) {
            dp[i] = 2 * dp[i - 2] % MOD;
            // 左边划分为j个人，右边则有i-j-2个人（需要除去划分的握手方式所关联的两个人）
            for (int j = 2; j <= i - 4; j += 2) {
                dp[i] = (dp[i] + dp[j] * dp[i - j - 2]) % MOD;
            }
        }
        return (int)dp[num_people];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：34ms，在所有java提交中击败了80.00%的用户。

内存消耗：35.4MB，在所有java提交中击败了74.60%的用户。

## 官方解题

&emsp;暂无，密切关注。社区有使用卡特兰数公式直接计算的思路。