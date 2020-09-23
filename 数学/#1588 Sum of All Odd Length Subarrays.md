[toc]

Given an array of positive integers `arr`, calculate the sum of all possible odd-length subarrays.

A subarray is a contiguous subsequence of the array.

Return the sum of all odd-length subarrays of `arr`.



**Constraints:**

- $1 \le \text{arr.length} \le 100$
- $1 \le \text{arr[i]} \le 1000$



## 题目解读

&emsp;求长度为奇数的所有子数组和。

```java
class Solution {
    public int sumOddLengthSubarrays(int[] arr) {

    }
}
```

## 程序设计

* 先求前缀和，然后遍历奇数长度。

```java
class Solution {
    public int sumOddLengthSubarrays(int[] arr) {
        int n = arr.length;
        int[] preSum = new int[n + 1];
        for (int i = 1; i <= n; i++) preSum[i] = preSum[i - 1] + arr[i - 1];

        // 遍历求和
        int sum = 0;
        for (int len = 1; len <= n; len += 2) {
            for (int i = 0, j = i + len - 1; j < n; i++, j++) {
                sum += preSum[j + 1] - preSum[i];
            }
        }
        return sum;
    }
}
```

* 如果以排列组合的思路考虑，问题变为包含当前值的组合数，对于长度为奇数的子数组，其左边可为偶数长，右边则为偶数长，或者左侧为奇数长，右边为奇数长。
* 对于偶数长的选择，不能忘记长度为$0$的情况，故偶数长的选择数为$\frac{n}{2} + 1$。

```java
class Solution {
    public int sumOddLengthSubarrays(int[] arr) {
        int n = arr.length;
        int sum = 0;
        for (int i = 0; i < n; i++) {
            // 左侧有i个，右侧有n-i-1个
            // 左侧选择奇数长度和偶数长度的数目
            int lOdd = (i + 1) / 2, lEven = i / 2 + 1;
            // 右侧同理
            int rOdd = (n - i) / 2, rEven = (n - i - 1) / 2 + 1;
            sum += (lOdd * rOdd + lEven * rEven) * arr[i];
        }
        return sum;
    }
}
```

## 性能分析

&emsp;前缀和思路时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了83.70%的用户。

内存消耗：37.1MB，在所有java提交中击败了5.19%的用户。

&emsp;排列组合思路时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.8MB，在所有java提交中击败了40.74%的用户。

## 官方解题

&emsp;暂无，密切关注。