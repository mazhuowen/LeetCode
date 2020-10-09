[toc]

Find the smallest prime palindrome greater than or equal to $N$.

Recall that a number is prime if it's only divisors are $1$ and itself, and it is greater than $1$. 

For example, $2$,$3$,$5$,$7$,$11$ and $13$ are primes.

Recall that a number is a palindrome if it reads the same from left to right as it does from right to left. 

For example, $12321$ is a palindrome.



**Note**:

* $1 \le N \le 10^8$
* The answer is guaranteed to exist and be less than $2 * 10^8$.



## 题目解读

&emsp;找到大于等于给定数字的最小回文素数。

```java
class Solution {
    public int primePalindrome(int N) {

    }
}
```

## 程序设计

* 最基本的思路是根据数字来回溯。

```java
class Solution {
    char[] nums;

    public int primePalindrome(int N) {
        if (N == 1) return 2;
        nums = Integer.toString(N).toCharArray();
        int len = getLen(N);
        int res = -1, l = len;
        while (res == -1 && l <= 9) {
            res = backTracing( new char[l], 0, N, len == l);
            l++;
        }
        return res;
    }

    // lead表示紧贴数字边界
    private int backTracing(char[] seq, int i, int N, boolean lead) {
        int res = -1;
        // 结束条件
        if (i > seq.length / 2) {
            res = transToInt(seq);
            // 满足条件，返回
            if (isPrime(res) && res >= N) return res;
            else return -1;
        }
        // 试探
        char c = lead ? nums[i] : '0';
        for (; c <= '9'; c++) {
            // 高位不能为0
            if (i == 0 && c == '0') continue;
            seq[i] = seq[seq.length - i - 1] = c;
            if ((res = backTracing(seq, i + 1, N, lead && nums[i] == c)) != -1) return res;
        }
        return res;
    }

    private int getLen(int N) {
        int res = 0;
        while (N > 0) {
            N /= 10;
            res++;
        }
        return res;
    }

    private int transToInt(char[] seq) {
        int num = 0;
        for (char c : seq) num = num * 10 + (c - '0');
        return num;
    }

    private boolean isPrime(int num) {
        int p = 2, limit = (int)Math.sqrt(num);
        while (p <= limit) {
            if (num % p == 0) return false;
            p++;
        }
        return true;
    }
}
```

## 性能分析

&emsp;粗略估计时间复杂度为$O(10^{\frac{L}{2}})$，空间复杂度为$O(L)$。

执行用时：12ms，在所有java提交中击败了90.86%的用户。

内存消耗：35.7MB，在所有java提交中击败了65.27%的用户。

## 官方解题

&emsp;官方提供了利用回文根遍历回文并判断的思路，存在大量重复判断。

```java
class Solution {
    public int primePalindrome(int N) {
        // 数字长度
        for (int L = 1; L <= 5; ++L) {
            // 根据回文根判断奇数长度的回文
            for (int root = (int)Math.pow(10, L - 1); root < (int)Math.pow(10, L); ++root) {
                // 生成回文
                StringBuilder sb = new StringBuilder(Integer.toString(root));
                for (int k = L-2; k >= 0; --k) sb.append(sb.charAt(k));
                
                // 判断是否是素数
                int x = Integer.valueOf(sb.toString());
                if (x >= N && isPrime(x)) return x;
            }

            // 根据回文根判断偶数长度的回文
            for (int root = (int)Math.pow(10, L - 1); root < (int)Math.pow(10, L); ++root) {
                // 生成回文
                StringBuilder sb = new StringBuilder(Integer.toString(root));
                for (int k = L-1; k >= 0; --k) sb.append(sb.charAt(k));
                int x = Integer.valueOf(sb.toString());
                // 判断是否是素数
                if (x >= N && isPrime(x)) return x;
            }
        }

        throw null;
    }

    public boolean isPrime(int N) {
        if (N < 2) return false;
        int R = (int) Math.sqrt(N);
        for (int d = 2; d <= R; ++d)
            if (N % d == 0) return false;
        return true;
    }
}
```

执行用时：60ms，在所有java提交中击败了63.12%的用户。

内存消耗：38.5MB，在所有java提交中击败了15.97%的用户。