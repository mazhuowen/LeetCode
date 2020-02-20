[toc]

Given a positive integer num, write a function which returns True if num is a perfect square else False.

Note: Do not use any built-in library function such as sqrt.



## 题目解读

&emsp;判断给定的数是否是整数的平方。

```java
class Solution {
    public boolean isPerfectSquare(int num) {
        int 
    }
}
```

## 程序设计

* 同求平方根，二分法（切记需要转为long类型）：

```java
class Solution {
    public boolean isPerfectSquare(int num) {
        if(num == 1) {
            return true;
        }
        int left = 1, right = num / 2;
        long squa;
        while(left <= right) {
            int mid = left + (right - left) / 2;
            squa = (long)mid * mid;
            if(squa == num) {
                return true;
            }
            if(squa < num) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return false;
    }
}
```

* 同求平方根，牛顿法：

```java
class Solution {
    public boolean isPerfectSquare(int num) {
        if(num == 1) {
            return true;
        }
        long t = num / 2;
        // 由于是整数，此处条件为大于而不取绝对值
        while(t*t > num) {
            t = (t + num / t) / 2;
        }
        if(t * t == num) {
            return true;
        }
        return false;
    }
}
```

> 注意两种方法都需要将乘积转化为长整形；牛顿法循环条件为`t*t > num`，而不是两者之差的绝对值大于等于1，因为此处是整数，假设$t_0^2 = num$，而初始$t^2 \ge num$，即$t \ge t_0$；在后续迭代中$t = (t + \frac{num}{t})/2 = \frac{1}{2}(\frac{t^2 + t_0^2}{t})$，则新的$t - t_0 = \frac{1}{2}(\frac{t^2 + t_0^2 - 2t \times t_0}{t}) \ge 0$，即初始时$t \ge t_0$，后续迭代$t \ge t_0$，所以条件是`t*t > num`，当小于等于时就已经得到结果了。如果此处用绝对值，就会造成死循环。

## 性能分析

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.6MB，在所有java提交中击败了5.12%的用户。

## 官方解题

&emsp;同上。