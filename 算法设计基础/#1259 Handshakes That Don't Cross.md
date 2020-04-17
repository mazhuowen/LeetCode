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

* 根据上述思路可知不相交握手方案必然是嵌套或拼接的，这样问题可分解为$i$个人前$j$个一组的握手方案和剩余人的握手方案之积；特别需要注意嵌套情况，即第一个人与最后一个人握手，其余人一组，仍然是特殊的拼接方案，为了动态实现的方便，用`dp(0)`表示第一个人和最后一个人的握手方案，为1。这样得到动态规划形式：

```java
class Solution {
    int mod = 1_000_000_007;

    public int numberOfWays(int num_people) {
        if (num_people <= 0 || num_people % 2 != 0) throw new IllegalArgumentException("invalid param");

        // 表示i对人的握手数
        long[] dp = new long[num_people / 2 + 1];
        // 初始化，0对人握手数为1，为了动态规划计算设置，代表第一个人和最后一个人握手的方案
        dp[0] = 1;
        // 两个人的握手数为1
        dp[1] = 1;

        for (int i = 2; i <= num_people / 2; i++) {
            for (int j = 0; j < i; j++) {
                // 前j对人握手乘剩下的人握手的次数
                // 特别注意j=0表示第一个人与最后一个人握手，中间剩余的人握手的方案
                dp[i] += dp[j] * dp[i - j - 1];
                dp[i] %= mod;
            }
        }
        return (int)dp[num_people / 2];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：36ms，在所有java提交中击败了69.74%的用户。

内存消耗：36.5MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区有使用卡特兰数公式直接计算的思路。