[toc]

Initially on a notepad only one character `'A'` is present. You can perform two operations on this notepad for each step:

* `Copy All`: You can copy all the characters present on the notepad (partial copy is not allowed).
* `Paste`: You can paste the characters which are copied **last time**.


Given a number n. You have to get **exactly** $n$ `'A'` on the notepad by performing the minimum number of steps permitted. Output the minimum number of steps to get $n$ `'A'`.



**Note:**

* The $n$ will be in the range `[1, 1000]`.



## 题目解读

&emsp;在一个写字板上只能拷贝、复制字母。求最后有$n$个字母的最少操作次数。

```java
class Solution {
    public int minSteps(int n) {

    }
}
```

## 程序设计

* 对于复制过程可抽象为`CPPPCPP`，对于其中的一段`CPPP`，假设起始字符长度为$m$，则复制过后总的字符长度就是$m + m * (l - 1) = m * l$，其中$l$为这段的长度；假设有$s$段这样的复制，则可知$\prod_{i = 1}^sl_i = n$；而最后的操作数就是$\sum_{i = 1}^sl_i$；
* 例如`12`，可分解为$2 * 6$和$3 * 4$和$2 * 2 * 3$，由于对于任一个可分解的数，分解后其因子和小于风雨原来值，故最少的步数就是将$n$素因子分解的结果。

```java
class Solution {
    public int minSteps(int n) {
        if (n < 1) throw new IllegalArgumentException("invalid param");
        int res = 0, d = 2;
        while (n > 1) {
            // 因子整除
            while (n % d == 0) {
                n /= d;
                res += d;
            }
            // 因子迭代
            d++;
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\sqrt{N})$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：35.4MB，在所有java提交中击败了92.44%的用户。

## 官方解题

&emsp;官方思路同上。