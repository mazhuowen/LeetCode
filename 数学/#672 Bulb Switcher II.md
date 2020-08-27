[toc]

There is a room with $n$ lights which are turned on initially and $4$ buttons on the wall. After performing exactly $m$ unknown operations towards buttons, you need to return how many different kinds of status of the $n$ lights could be.

Suppose $n$ lights are labeled as number $[1, 2, 3, \dots, n]$, function of these $4$ buttons are given below:

* Flip all the lights.
* Flip lights with even numbers.
* Flip lights with odd numbers.
* Flip lights with $(3k + 1)$ numbers, $k = 0, 1, 2, \dots$



**Note**: $n$ and $m$ both fit in range $[0, 1000]$.



## 题目解读

&emsp;有$n$个灯泡和$4$个开关，返回$m$次操作后所有可能的灯泡状态。

```java
class Solution {
    public int flipLights(int n, int m) {

    }
}
```

## 程序设计

* 列举前$6$个灯的状态，假设四个开关的操作数为$a$、$b$、$c$、$d$，则：

$$
Light 1 = (1 + a + c + d) \mod 2\\
Light 2 = (1 + a + b) \mod 2\\
Light 3 = (1 + a + c) \mod 2\\
Light 4 = (1 + a + b + d) \mod 2\\
Light 5 = (1 + a + c) \mod 2\\
Light 6 = (1 + a + b) \mod 2
$$

* 仔细观察规律，发现前$3$个灯确定后续灯的状态，因为$1 + a + c + d$为偶数时，$1 + a + b + d$的奇偶性也会得到确认，因为$a + b + c + d = m$。这样分析前三个灯的所有情况，可得：

```java
class Solution {
    public int flipLights(int n, int m) {
        n = Math.min(n, 3);
        // 全部打开
        if (m == 0) return 1;
        // 一个操作，根据n的数目返回答案
        if (m == 1) return n == 1 ? 2 : n == 2 ? 3 : 4;
        // 两个操作
        if (m == 2) return n == 1 ? 2 : n == 2 ? 4 : 7;
        // 多于三个操作，可得到所有的组合数
        return n == 1 ? 2 : n == 2 ? 4 : 8;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.7MB，在所有java提交中击败了15.38%的用户。

## 官方解题

&emsp;见上。