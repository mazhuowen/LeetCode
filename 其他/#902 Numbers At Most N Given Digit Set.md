[toc]

Given an array of `digits` which is sorted in **non-decreasing** order. You can write numbers using each `digits[i]` as many times as we want. For example, if `digits = ['1','3','5']`, we may write numbers such as `'13'`, `'551'`, and `'1351315'`.

Return the number of positive integers that can be generated that are less than or equal to a given integer $n$.

 

**Example 1**:

```
Input: digits = ["1","3","5","7"], n = 100
Output: 20
Explanation: 
The 20 numbers that can be written are:
1, 3, 5, 7, 11, 13, 15, 17, 31, 33, 35, 37, 51, 53, 55, 57, 71, 73, 75, 77.
```

**Example 2**:

```
Input: digits = ["1","4","9"], n = 1000000000
Output: 29523
Explanation: 
We can write 3 one digit numbers, 9 two digit numbers, 27 three digit numbers,
81 four digit numbers, 243 five digit numbers, 729 six digit numbers,
2187 seven digit numbers, 6561 eight digit numbers, and 19683 nine digit numbers.
In total, this is 29523 integers that can be written using the digits array.
```

**Example 3**:

```
Input: digits = ["7"], n = 8
Output: 1
```



**Constraints**:

* $1 \le \text{digits.length} \le 9$
* $\text{digits[i].length} == 1$
* `digits[i]` is a digit from `'1'` to `'9'`.
* All the values in `digits` are **unique**.
* `digits` is sorted in **non-decreasing** order.
* $1 \le n \le 10^9$



## 题目解读

&emsp;统计给定数字字符可构成的小于$n$的数字数目。

```java
class Solution {
    public int atMostNGivenDigitSet(String[] digits, int n) {

    }
}
```

## 程序设计

* 使用数位动态规划来解决，`dp(pos,stat,lead,limit)`表示当前数字的位数（从最高位算起）、状态、前导零、限制；
* 对于状态，只需记录前一个位置的字符即可；

```java
class Solution {
    int[][][][] record;
    char[] high;

    public int atMostNGivenDigitSet(String[] digits, int n) {
        this.high = Integer.toString(n).toCharArray();
        this.record = new int[high.length][10][2][2];
        // 初始化
        for (int[][][] arrs : record) for (int[][] arr : arrs) for (int[] a : arr) Arrays.fill(a, -1);

        // 减去0
        return backTracing(0, '0', true, true, digits) - 1;
    }

    private int backTracing(int pos, char c, boolean lead, boolean limit, String[] digits) {
        // 找到一个解
        if (pos == high.length) return 1;
        if (record[pos][c - '0'][lead ? 1 : 0][limit ? 1 : 0] != -1) return record[pos][c - '0'][lead ? 1 : 0][limit ? 1 : 0];

        int res = 0;
        // 如果前导零，则可继续前导零
        if (lead) res += backTracing(pos + 1, '0', lead, false, digits);
        // 选择当前位置的字符
        char h = limit ? high[pos] : '9';
        for (String ch : digits) {
            char cur = ch.charAt(0);
            if (cur > h) break;
            res += backTracing(pos + 1, cur, false, limit && cur == h, digits);
        }
        record[pos][c - '0'][lead ? 1 : 0][limit ? 1 : 0] = res;
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(Len)$，空间复杂度为$O(Len)$。$Len$为给定个数字的长度。

执行用时：2 ms, 在所有 Java 提交中击败了17.12%的用户

内存消耗：36.1 MB, 在所有 Java 提交中击败了45.28%的用户。

## 官方解题

&emsp;官方采用数位动态规划的思路，实际上只需考虑长度等于$N$的符合条件的数，因为长度小于$N$的程度，则可以任选数组给定的数字组成，假设长度为$K$，则数目为$D^K$；同时简化数位动态规划数组，使用`dp(i)`表示最低位截止$i$位的符合条件的数字。如`2345`对应的`dp(2)`表示小于等于`45`的满足条件的数字。

```java
class Solution {
    public int atMostNGivenDigitSet(String[] digits, int n) {
        char[] high = Integer.toString(n).toCharArray();
        int len = high.length;
        int[] dp = new int[len + 1];
        dp[len] = 1;
        // 从低位往高位遍历
        for (int i = len - 1; i >= 0; i--) {
            char limit = high[i];
            for (String digit : digits) {
                char d = digit.charAt(0);
                if (limit < d) break;
                if (limit > d) dp[i] += (int)Math.pow(digits.length, len - i - 1);
                else dp[i] += dp[i + 1];
            }
        }
        int res = dp[0];
        // 计算长度小于len的数字组合
        for (int i = len - 1; i >= 1; i--) {
            res += (int)Math.pow(digits.length, i);
        }
        return res;
    }
}
```

&emsp;时间复杂度为$O(Len)$，空间复杂度为$O(Len)$。

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：36.1 MB, 在所有 Java 提交中击败了45.28%的用户。