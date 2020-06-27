[toc]

Given two integer arrays `A` and `B`, return the maximum length of an subarray that appears in both arrays.



**Note:**

* $1 \le \text{len(A), len(B)} \le 1000$
* $0 \le \text{A[i], B[i]} < 100$



## 题目解读

&emsp;返回两个数组的最长公共子数组长度。

```java
class Solution {
    public int findLength(int[] A, int[] B) {

    }
}
```

## 程序设计

* 采用暴力遍历超时。使用动态规划数组`dp(i,j)`表示数组`A`的$0 \sim i$部分与数组`B`的$0 \sim j$部分公共结尾的长度；则只需在遍历的过程中记录最大长度。

```java
class Solution {
    public int findLength(int[] A, int[] B) {
        if (A == null || A.length == 0 || B == null || B.length == 0) return 0;

        int m = A.length, n = B.length;
        int maxLen = 0;
        // 表示0~i与0~j结尾的数组的公共长度
        int[][] dp = new int[m + 1][n + 1];

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // 如果结尾相等，则长度加一，否则长度为0
                if (A[i] == B[j]) dp[i + 1][j + 1] = dp[i][j] + 1;

                maxLen = Math.max(maxLen, dp[i + 1][j + 1]);
            }
        }
       return maxLen;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：56ms，在所有java提交中击败了63.86%的用户。

内存消耗：48.7MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;除了上述思路，官方还提供了借鉴字符串匹配RK加二分查找的思路。使用`Rabin-Karp`算法求出一个序列的哈希值。具体地，指定一个素数$p$，那么序列`S` 的哈希值为$\text{hash}(S) = \sum_{i=0}^{\vert S\vert-1} p^i * S[i]$。由于这个值一般会非常大，因此会将它对另一个素数$M$取模。当在一个序列`S`中算出所有长度为$l$的子序列的哈希值时，可以用类似滑动窗口的方法，在线性时间内得到这些子序列的哈希值。例如$S[1:l+1] = \frac{S[0:l] - S[0]}{p} + p^{n-1}*S[l]$。由于分子上的$S[0:l] - S[0]$已经对`M`取模，我们无法知道它除以$p$的值，因此需要使用`Fermat`小定理的推论：
$$
p^{-1} \equiv p^{M-2}\pmod{M}
$$
将除法变为乘法，这样原式变为$S[1:l+1] = (S[0:l] - S[0]) * p^{M-2} + p^{n-1}*S[l]$。为了保证百分百的正确性，当两个字符串的哈希值相等时，还要判断它们对应的字符串是否相等，防止哈希碰撞。

```java
import java.math.BigInteger;

class Solution {
    // 选择大于100的素数
    int P = 113;
    // 取模
    int MOD = 1_000_000_007;
    // P^(M - 2)
    int Pinv = BigInteger.valueOf(P).modInverse(BigInteger.valueOf(MOD)).intValue();

    public int findLength(int[] A, int[] B) {
        if (A == null || B == null || A.length == 0 || B.length == 0) return 0;

        int m = A.length, n = B.length;
        int left = 0, right = Math.min(m, n);

		// 二分查找
        while (left < right) {
            int mid = left + (right - left) / 2 + 1;
            // 存在mid长度的相同子数组
            if (checkLen(A, B, mid)) left = mid;
            else right = mid - 1;
        }
        return left;
    }

    private boolean checkLen(int[] A, int[] B, int len) {
        int[] hashA = hash(A, len);
        int[] hashB = hash(B, len);

		// 记录，空间换取时间
        Map<Integer, List<Integer>> hashRecord = new HashMap<>();
        for (int idx = 0; idx < hashA.length; idx++) {
            if (hashRecord.get(hashA[idx]) == null) hashRecord.put(hashA[idx], new ArrayList<>());
            hashRecord.get(hashA[idx]).add(idx);
        }

		// 比较
        for (int idx1 = 0; idx1 < hashB.length; idx1++) {
            if (hashRecord.get(hashB[idx1]) == null) continue;
            for (int idx2 : hashRecord.get(hashB[idx1])) {
                if (Arrays.equals(Arrays.copyOfRange(B, idx1, idx1 + len), Arrays.copyOfRange(A, idx2, idx2 + len))) return true;
            }
        }
        return false;
    }

    private int[] hash(int[] nums, int len) {
        int[] res = new int[nums.length - len + 1];
        long hash = 0L, power = 1L;
        int idx = 0;
        // 定位窗口
        for (int i = 0; i < len; i++) {
            hash = (hash + nums[i] * power) % MOD;
            if (i < len - 1) power = (power * P) % MOD;
        }
        res[idx++] = (int)hash;
        // 滑动窗口
        for (int i = len; i < nums.length; i++) {
            hash = (hash - nums[i - len]) * Pinv % MOD + MOD + nums[i] * power;
            hash %= MOD;
            
            res[idx++] = (int)hash;
        }

        return res;
    }
}
```

&emsp;时间复杂度为$O(\max(M,N)\log_2(\min(M,N)))$，空间复杂度为$O(\max(M,N))$。

执行用时：26ms，在所有java提交中击败了98.87%的用户。

内存消耗：40.2MB，在所有java提交中击败了100.00%的用户。