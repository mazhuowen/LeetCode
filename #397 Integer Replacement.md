[toc]

Given a positive integer $n$ and you can do operations as follow:

* If n is even, replace n with n/2.
* If n is odd, you can replace n with either n + 1 or n - 1.

What is the minimum number of replacements needed for n to become 1?



## 题目解读

&emsp;返回$n$转换为$1$的最小次数。

```java
class Solution {
    public int integerReplacement(int n) {

    }
}
```

## 程序设计



```java
class Solution {
    public int integerReplacement(int n) {
        if (n <= 0) throw new IllegalArgumentException("invalid param");

        if (n == Integer.MAX_VALUE) return 32;

        int count = 0;
        while (n > 1) {
            // 奇数
            if ((n & 1) == 1) {
                if (n > 3 && (n & 3) == 3) n += 1;
                else n -= 1;
            }
            // 偶数除二
            else {
                n >>= 1;
            }
            count++;
        }
        return count;
    }
}
```

## 性能分析



## 官方解题
