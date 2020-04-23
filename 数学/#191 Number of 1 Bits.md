[toc]

Write a function that takes an unsigned integer and return the number of `'1'` bits it has (also known as the Hamming weight).



Note:

* Note that in some languages such as Java, there is no unsigned integer type. In this case, the input will be given as signed integer type and should not affect your implementation, as the internal binary representation of the integer is the same whether it is signed or unsigned.
* In Java, the compiler represents the signed integers using `2`'s complement notation. Therefore, in **Example 3** above the input represents the signed integer `-3`.



## 题目解读

&emsp;输出二进制数的1的数目。

```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        
    }
}
```

## 程序设计

* 整数移位操作。

```java
public class Solution {
    public int hammingWeight(int n) {
        int count = 0;
        for (int i = 0; i < 32; i ++) {
            count += (n & 1);
            n >>= 1;
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了99.54%的用户。

内存消耗：36.8MB，在所有java提交中击败了5.26%的用户。

## 官方解题

&emsp;同上。