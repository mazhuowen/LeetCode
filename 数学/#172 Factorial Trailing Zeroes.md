[toc]

Given an integer $n$, return the number of trailing zeroes in $n!$.



**Note:** Your solution should be in logarithmic time complexity.



## 题目解读

&emsp;求解阶乘尾部连续的零的数目。

```java
class Solution {
    public int trailingZeroes(int n) {

    }
}
```

## 程序设计

* 对于小于5的阶乘，结尾没有0；$5!$阶乘末尾存在一个0，由$5 * 2$产生；小于10大于5的阶乘仍然只有一个0；$10!$有两个0，分别由$10$及$5 * 2$产生；可见0的数目与5这个周期有关。
* 对于阶乘的任意一段5的周期，记为$5i * (5i - 1) * (5i - 2)*(5i - 3) * (5i - 4)$，$i$为偶数则0在$5i$中产生，其它四个数不存在0或5结尾，无法构成0；$i$为奇数则0在$5i$和前面偶数中的2相乘产生。每个周期只有一个0。阶乘末尾的0的数目就是5的周期的数目。

```java
class Solution {
    public int trailingZeroes(int n) {
        int count = 0;
        while (n > 0) {
            count += (n /= 5);
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_5N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了99.66%的用户。

内存消耗：36.9MB，在所有java提交中击败了5.55%的用户。

## 官方解题

&emsp;暂无，密切关注。