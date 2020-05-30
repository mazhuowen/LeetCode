[toc]

Your car starts at position 0 and speed +1 on an infinite number line.  (Your car can go into negative positions.)

Your car drives automatically according to a sequence of instructions A (accelerate) and R (reverse).

When you get an instruction "A", your car does the following: `position += speed, speed *= 2`.

When you get an instruction `"R"`, your car does the following: if your speed is positive then `speed = -1` , otherwise `speed = 1`.  (Your position stays the same.)

For example, after commands `"AAR"`, your car goes to positions `0->1->3->3`, and your speed goes to `1->2->4->-1`.

Now for some target position, say the **length** of the shortest sequence of instructions to get there.



**Note:**

- $1 \le target \le 10000$.



## 题目解读

&emsp;现在给定一个目标位置，请给出能够到达目标位置的最短指令列表的长度。

```java
class Solution {
    public int racecar(int target) {

    }
}
```

## 程序设计

* 使用动态规划数组`dp(i)`表示到达`i`的最短指令，如果$i = 2^n - 1$，则正好为$n$个`A`指令；如果不是则$2^{n - 1} \le i < 2^n - 1$，此时分为两种情况：

  * $n$个`A`指令到达`i`的后面，然后`R`折返剩余路程，即$dp(i) = n + 1 + dp(2^n - 1 - i)$；
  * $n - 1$个`A`到达`i`的前面，然后`R`折返`j`个`A`指令，然后再次`R`折返剩余路程，即$dp(i) = n - 1 + 2 + j + dp(i + 2^j - 1 - 2^(n - 1) + 1)$。

  尝试上述方案，找到最小值即可。

```java
class Solution {
    public int racecar(int target) {
        if (target < 1) throw new IllegalArgumentException("invalid param");

        // 动态规划数组，表示到达i的最短指令
        int[] dp = new int[target + 3];
        Arrays.fill(dp, Integer.MAX_VALUE);
        // 初始化，i为2时最短指令为AARA
        dp[0] = 0; dp[1] = 1; dp[2] = 4;

        for (int i = 3; i <= target; i++) {
            int k = getPower(i);
            int lower = 1 << (k - 1), upper = (1 << k) - 1;
            // 正好等于2^n-1，则只需要n个A指令
            if (i == upper) dp[i] = k;
            else {
                // 方案1，到达2^n-1处（超过target），然后R折返，需要k个A、1个R和剩余折返的长度
                dp[i] = k + 1 + dp[upper - i];
                // 方案2，到达2^(n-1)-1处，然后R折返j个A，然后再次R折返
                for (int j = 0; j < k - 1; j++) {
                    // 需要k-1个A，1个R和j个A及其剩余的指令数目
                    dp[i] = Math.min(dp[i], k - 1 + 2 + j + dp[i - lower + 1 + (1 << j) - 1]);
                }
            }
        }

        return dp[target];
    }

    // 返回小于等于输入的最大的二次方指数
    private int getPower(int num) {
        return 32 - Integer.numberOfLeadingZeros(num);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：5ms，在所有java提交中击败了80.24%的用户。

内存消耗：36.6MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;上述思路参考官方。