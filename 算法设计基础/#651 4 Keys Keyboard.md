[toc]

Imagine you have a special keyboard with the following keys:

* `Key 1: (A)`: Print one 'A' on screen.
* `Key 2: (Ctrl-A)`: Select the whole screen.
* `Key 3: (Ctrl-C)`: Copy selection to buffer.
* `Key 4: (Ctrl-V)`: Print buffer on screen appending it after what has already been printed.

Now, you can only press the keyboard for **N** times (with the above four keys), find out the maximum numbers of 'A' you can print on screen.



**Note:**

* $1 \le N \le 50$
* Answers will be in the range of 32-bit signed integer.



## 题目解读

&emsp;假设按键1在屏幕上打印一个`A`；按键2选中整个屏幕；按键3复制选中区域到缓冲区；按键4将缓冲区内容输出到上次输入的结束位置，并显示在屏幕上。求按键$N$次最多可显示的`A`的数目。

```java
class Solution {
    public int maxA(int N) {

    }
}
```

## 程序设计

* 对于每一步，都可以通过在前面的基础上继续拼写`A`，或者通过多次复制，取其中最大的方案。

```java
class Solution {
    public int maxA(int N) {
        if (N < 1) return 0;

        int[] dp = new int[N + 1];
        for (int i = 1; i <= N; i++) {
            //　在前一序列的基础上继续拼写Ａ
            dp[i] = dp[i - 1] + 1;

            // ctrl a ctrl c 和i - j - 1个ctrl v复制
            for (int j = i - 3; j >= 3; j--)
                dp[i] = Math.max(dp[i], dp[j] * (i - j - 1));
        }
        return dp[N];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了93.23%的用户。

内存消耗：36.2MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方在上述基础上继续优化。假设连续复制的次数是$n$，则复制后的字符数为$na$；假设连续复制$n - 3$，最后再复制一次，则复制后的数目为$2(n - 3)a$，可见当$n \ge 6$时，后一种方案数目更大，即连续复制不超过$5$次，这样可以优化内层循环。

```java
class Solution {
    public int maxA(int N) {
        if (N < 1) return 0;

        int[] dp = new int[N + 1];
        for (int i = 1; i <= N; i++) {
            //　在前一序列的基础上继续拼写Ａ
            dp[i] = dp[i - 1] + 1;

            // 复制不超过５次
            for (int j = i - 3; j >= i - 7 && j >= 0; j--)
                dp[i] = Math.max(dp[i], dp[j] * (i - j - 1));
        }
        return dp[N];
    }
}
```

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.1MB，在所有java提交中击败了100.00%的用户。