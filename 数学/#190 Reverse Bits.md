[toc]

Reverse bits of a given 32 bits unsigned integer.



Note:

* Note that in some languages such as Java, there is no unsigned integer type. In this case, both input and output will be given as signed integer type and should not affect your implementation, as the internal binary representation of the integer is the same whether it is signed or unsigned.
* In Java, the compiler represents the signed integers using 2's complement notation. Therefore, in Example 2 above the input represents the signed integer -3 and the output represents the signed integer -1073741825.



## 题目解读

&emsp;翻转二进制数字。

```java
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        
    }
}
```

## 程序设计

* 位操作。

```java
public class Solution {
    Map<Integer, Integer> record = new HashMap<>();
    
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        if (record.containsKey(n)) return record.get(n);

        int origin = n, reverse = 0;
        for (int i = 0; i < 32; i++) {
            reverse <<= 1;
            reverse |= (n & 1);
            n >>= 1;
        }
        record.put(origin, reverse);
        record.put(reverse, origin);
        return reverse;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(N)$，$N$为操作次数。

执行用时：2ms，在所有java提交中击败了14.38%的用户。

内存消耗：39.6MB，在所有java提交中击败了7.14%的用户。

## 官方解题

&emsp;官方除了最基本的循环思路，还提供了分治的思路。