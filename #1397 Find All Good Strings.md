[toc]

Given the strings `s1` and `s2` of size $n$, and the string `evil`. Return the number of **good** strings.

A **good** string has size $n$, it is alphabetically greater than or equal to `s1`, it is alphabetically smaller than or equal to `s2`, and it does not contain the string `evil` as a substring. Since the answer can be a huge number, return this modulo $10^9 + 7$.



**Constraints**:

* $\text{s1.length} == n$
* $\text{s2.length} == n$
* $s_1 \le s_2$
* $1 \le n \le 500$
* $1 \le \text{evil.length} \le 50$
* All strings consist of lowercase English letters.



## 题目解读

&emsp;好字符串定义为长度为$n$，字典序大于等于`s1`，小于等于`s2`，且不包含`evil`得字符串。求给定`s1`和`s2`间得所有好字符数目。

```java
class Solution {
    public int findGoodStrings(int n, String s1, String s2, String evil) {

    }
}
```

## 程序设计

* 

```java

```

## 性能分析

&emsp;



## 官方解题

&emsp;