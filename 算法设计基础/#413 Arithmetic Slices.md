[toc]

`A` sequence of number is called arithmetic if it consists of at least three elements and if the difference between any two consecutive elements is the same.

A zero-indexed array `A` consisting of N numbers is given. A slice of that array is any pair of integers `(P, Q)` such that `0 <= P < Q < N`.

A slice `(P, Q)` of array `A` is called arithmetic if the sequence:
`A[P]`, `A[p + 1]`, ..., `A[Q - 1]`, `A[Q]` is arithmetic. In particular, this means that `P + 1 < Q`.

The function should return the number of arithmetic slices in the array `A`.



## 题目解读

&emsp;寻找数组中等差数列子数组数目。

```java
class Solution {
    public int numberOfArithmeticSlices(int[] A) {

    }
}
```

## 程序设计

* 参考官方解题，采用动态规划数组`dp(i)`表示以`i`位置值结尾的等差数列个数，则如果`i + 1`与`i`、`i - 1`构成等差数列，`dp(i + 1) = dp(i) + 1`。

```java
class Solution {
    public int numberOfArithmeticSlices(int[] A) {
        if (A == null || A.length < 3) return 0;

        int count = 0;
        int[] dp = new int[A.length];
        for (int i = 2; i < A.length; i++) {
            // 当前元素与前一区间构成等差数列
            if (A[i] - A[i - 1] == A[i - 1] - A[i - 2]) {
                // 以A[i]结尾的等差数列为当前3个值及与A[i-1]中的等差数列之和
                dp[i] = dp[i - 1] + 1;
            }
            count += dp[i];
        }
        return count;
    }
}
```

> 由于只依赖前一个值，在空间上可继续简化。

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.2MB，在所有java提交中击败了16.67%的用户。

## 官方解题

&emsp;除了上述思路，官方还提供了数学方式：

```java
public class Solution {
    public int numberOfArithmeticSlices(int[] A) {
        int count = 0;
        int sum = 0;
        for (int i = 2; i < A.length; i++) {
            if (A[i] - A[i - 1] == A[i - 1] - A[i - 2]) {
                count++;
            } else {
                sum += (count + 1) * (count) / 2;
                count = 0;
            }
        }
        return sum += count * (count + 1) / 2;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.4MB，在所有java提交中击败了16.67%的用户。