[toc]

Given two positive integers `x` and `y`, an integer is powerful if it is equal to $x^i + y^j$ for some integers $i \ge 0$ and $j \ge 0$.

Return a list of all powerful integers that have value less than or equal to `bound`.

You may return the answer in any order.  In your answer, each value should occur at most once.

 

**Example 1**:

```
Input: x = 2, y = 3, bound = 10
Output: [2,3,4,5,7,9,10]
Explanation: 
2 = 2^0 + 3^0
3 = 2^1 + 3^0
4 = 2^0 + 3^1
5 = 2^1 + 3^1
7 = 2^2 + 3^1
9 = 2^3 + 3^0
10 = 2^0 + 3^2
```

**Example 2**:

```
Input: x = 3, y = 5, bound = 15
Output: [2,4,6,8,10,14]
```



**Note**:

* $1 \le x \le 100$
* $1 \le y \le 100$
* $0 \le \text{bound} \le 10^6$



## 题目解读

&emsp;给定小于等于`bound`的所有强整数。

```java
class Solution {
    public List<Integer> powerfulIntegers(int x, int y, int bound) {

    }
}
```

## 程序设计

* 循环遍历，需要注意$x = 1$和$y = 1$的情况，避免无限循环。

```java
class Solution {
    public List<Integer> powerfulIntegers(int x, int y, int bound) {
        Set<Integer> set = new HashSet<>();
        for (int base1 = 1; base1 > 0 && base1 < bound; base1 *= x) {
            for (int base2 = 1; base2 > 0 && base1 + base2 <= bound; base2 *= y) {
                set.add(base1 + base2);
                // 避免y为1无限循环
                if (y == 1) break;
            }
            // 避免x为1无限循环
            if (x == 1) break;
        }

        return new LinkedList<>(set);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(\log_2N)$。

执行用时：1 ms, 在所有 Java 提交中击败了100.00%的用户。

内存消耗：36.3 MB, 在所有 Java 提交中击败了67.75%的用户。

## 官方解题

&emsp;官方思路类似，暴力枚举。
