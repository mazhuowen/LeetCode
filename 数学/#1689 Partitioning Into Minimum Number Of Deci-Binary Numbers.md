[toc]

A decimal number is called **deci-binary** if each of its digits is either $0$ or $1$ without any leading zeros. For example, $101$ and $1100$ are deci-binary, while $112$ and $3001$ are not.

Given a string $n$ that represents a positive decimal integer, return the **minimum** number of positive **deci-binary** numbers needed so that they sum up to $n$.

 

**Example 1**:

```
Input: n = "32"
Output: 3
Explanation: 10 + 11 + 11 = 32
```

**Example 2**:

```
Input: n = "82734"
Output: 8
```

**Example 3**:

```
Input: n = "27346209830709182346"
Output: 9
```



**Constraints**:

* $1 \le \text{n.length} \le 10^5$
* $n$ consists of only digits.
* $n$ does not contain any leading zeros and represents a positive integer.



## 题目解读

&emsp;给定十进制数，求可逐位相加二进制数得到给定数的最小数目。

```java
class Solution {
    public int minPartitions(String n) {

    }
}
```

## 程序设计

* 由于二进制位数只能是$0$或$1$，则最多的二进制数目选择十进制最大的个位数。

```java
class Solution {
    public int minPartitions(String n) {
        int count = 0;
        for (char c : n.toCharArray()) {
            count = Math.max(count, c - '0');
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：6 ms, 在所有 Java 提交中击败了85.73%的用户。

内存消耗：39 MB, 在所有 Java 提交中击败了75.23%的用户。

## 官方解题

&emsp;暂无，密切关注。
