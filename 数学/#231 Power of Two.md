[toc]

Given an integer, write a function to determine if it is a power of two.



## 题目解读

&emsp;判断是否是2的幂次方。

```java
class Solution {
    public boolean isPowerOfTwo(int n) {

    }
}
```

## 程序设计

* 统计二进制是否只有一个1。

```java
class Solution {
    public boolean isPowerOfTwo(int n) {
        if (n <= 0) return false;

        int count = 0;
        while (n > 0) {
            if ((n & 1) == 1) count++;
            n >>= 1;
        }
        return count == 1;
    }
}
```

## 性能分析

&emsp;空间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.3MB，在所有java提交中击败了6.90%的用户。

## 官方解题

&emsp;官方解题提供另一种思路，2的幂次方除了最右侧有一个1，其余为0，通过按位与比较低位是否是0。

```java
class Solution {
    public boolean isPowerOfTwo(int n) {
        if (n == 0) return false;
    	long x = (long) n;
    	return (x & (x - 1)) == 0;
  	}
}
```

