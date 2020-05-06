[toc]

Given a non-negative integer `num`, repeatedly add all its digits until the result has only one digit.



**Follow up:**
Could you do it without any loop/recursion in $O(1)$ runtime?



## 题目解读

&emsp;将数字位数相加，重复这个过程直到数值为个位数。

```java
class Solution {
    public int addDigits(int num) {

    }
}
```

## 程序设计

* 需注意最终数值为个位数。

```java
class Solution {
    public int addDigits(int num) {
        if (num < 0) throw new IllegalArgumentException("invalid param");

        // 位数相加
        int sum = 0;
        while (num > 0) {
            sum += (num % 10);
            num /= 10;
        }
        // 继续相加
        if (sum > 9) return addDigits(sum);
        return sum;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.1MB，在所有java提交中击败了8.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区采用数学规律$num = a * 100 + b * 10 + c = a * 99 + b * 9 + a + b + c$，可知$num\ \%\ 9 = (a + b + c)\ \%\ 9$，对于特殊值如$9$、$0$等做处理，先减去$1$，取余后再加回来。

```java
class Solution {
    public int addDigits(int num) {
        if (num < 0) throw new IllegalArgumentException("invalid param");

        return (num - 1) % 9 + 1;
    }
}
```