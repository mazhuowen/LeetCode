[toc]

Given an integer `d` between `0` and `9`, and two positive integers `low` and `high` as lower and upper bounds, respectively. Return the number of times that `d` occurs as a digit in all integers between `low` and `high`, including the bounds `low` and `high`.



**Note:**

* $0 \le d \le 9$
* $1 \le low \le high \le 2×10^8$



## 题目解读

&emsp;给定数字，求在规定数字区间，该数字出现的数目。

```java
class Solution {
    public int digitsCount(int d, int low, int high) {

    }
}
```

## 程序设计

* 类似[#233 Number of Digit One](./#233 Number of Digit One.md)，在$d$不为$0$的情况下，仍可用如下思路完成，但当$d$为$0$时，该思路不再适合。

```java
private int digitsCount(int d, int num) {
    if (num < d) return 0;
    if (num < 10) return 1;

    int digit = 1, len = 1;
    while (num / digit >= 10) {
        digit *= 10;
        len++;
    }
    int high = num / digit;

    int count = 0;
    // 高位为d的个数
    if (high == d) count += num - high * digit + 1;
    else if (high > d) count += digit;
        
    // 高位固定，其后位为d的情况
    count += high * (len - 1) * (int)Math.pow(10, len - 2);
    return count + digitsCount(d, num % digit);
}
```

* 可参考[#233 Number of Digit One](./#233 Number of Digit One.md)的官方解题，仍然按位统计，对于0需要特殊处理，对于1，存在1、10、11、12、……，而对于0，则相应的会计算0、00、01、02、……，显然00、01、02等不存在，需要从统计数目里减去，其数目正好为当前位数个。

```java
class Solution {
    public int digitsCount(int d, int low, int high) {
        return digitsCount(d, high) - digitsCount(d, low - 1);
    }

    private int digitsCount(int d, int num) {
        long res = 0;
        for (long digit = 1; digit <= num; digit *= 10) {
            long base = digit * 10;
            res += num / base * digit + Math.min(Math.max(num % base - d * digit + 1, 0), digit);

            if (d == 0) res -= digit;
        }
        return (int)res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_{10}N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。