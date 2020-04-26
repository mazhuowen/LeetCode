[toc]

Calculate the sum of two integers a and b, but you are **not allowed** to use the operator `+` and `-`.



## 题目解读

&emsp;不使用加减实现加减操作。

```java
class Solution {
    public int getSum(int a, int b) {

    }
}
```

## 程序设计

* 通过异或和与操作得到当前位值和进位值，然后再继续调用加减方法，直到进位为0.

```java
class Solution {
    public int getSum(int a, int b) {
        int base = a ^ b;
        int carry = (a & b) << 1;
        if (carry == 0) return base;
        else return getSum(base, carry);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.4MB，在所有java提交中击败了8.00%的用户。

## 官方解题

&emsp;暂无，密切关注。