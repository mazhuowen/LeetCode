[toc]

Given a binary string `S` (a string consisting only of '0' and '1's) and a positive integer $N$, return true if and only if for every integer $X$ from $1$ to $N$, the binary representation of $X$ is a substring of `S`.



**Note:**

* $1 \le \text{S.length} \le 1000$
* $1 \le N \le 10^9$



## 题目解读

&emsp;给定一个二进制字符串，判断$1 \sim N$的二进制序列是否是其子字符串。

```java
class Solution {
    public boolean queryString(String S, int N) {
        
    }
}
```

## 程序设计

* 基本思路是遍历判断是否包含，直接使用工具类。

```java
class Solution {
    public boolean queryString(String S, int N) {
        if (S == null || S.isEmpty()) return false;
        
        for (int i = N; i >= 1; i--) {
            if (!S.contains(Integer.toBinaryString(i))) return false;
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.8MB，在所有java提交中击败了27.42%的用户。

## 官方解题

&emsp;暂无，密切关注。