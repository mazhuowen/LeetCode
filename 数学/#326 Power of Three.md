[toc]

Given an integer, write a function to determine if it is a power of three.



**Follow up**:
Could you do it without using any loop / recursion?



## 题目解读

&emsp;判断是否是$3$的幂次方。

```java
class Solution {
    public boolean isPowerOfThree(int n) {
        
    }
}
```

## 程序设计

* 最基本的思路整除。

```java
class Solution {
    public boolean isPowerOfThree(int n) {
        while (n > 1) {
            if (n % 3 != 0) return false;
            n /= 3; 
        }
        return n == 1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_3N)$，空间复杂度为$O(1)$。

执行用时：15ms，在所有java提交中击败了95.87%的用户。

内存消耗：39.6MB，在所有java提交中击败了8.70%的用户。

## 官方解题

&emsp;官方提供了无需循环和递归的思路，即整型最大数字为$3^{19} = 1162261467$，由于$3$是质数，其他$3$的幂次方都能被该数整除，故可取余判断。

```java
public class Solution {
    public boolean isPowerOfThree(int n) {
        return n > 0 && 1162261467 % n == 0;
    }
}
```

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：15ms，在所有java提交中击败了95.87%的用户。

内存消耗：39.6MB，在所有java提交中击败了8.70%的用户。