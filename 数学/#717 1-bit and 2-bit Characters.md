[toc]

We have two special characters. The first character can be represented by one bit `0`. The second character can be represented by two bits (`10` or `11`).

Now given a string represented by several bits. Return whether the last character must be a one-bit character or not. The given string will always end with a zero.



**Note:**

* $1 \le \text{len(bits)} \le 1000$.
* `bits[i]` is always `0` or `1`.



## 题目解读

&emsp;有两种编码格式，判断末尾的编码是否一定是单位编码。

```java
class Solution {
    public boolean isOneBitCharacter(int[] bits) {

    }
}
```

## 程序设计

* 发现规律，最后一位0前面必须有偶数个1时，必定为单位编码。

```java
class Solution {
    public boolean isOneBitCharacter(int[] bits) {
        if (bits == null || bits.length == 0) return false;

        int n = bits.length;
        if (bits[n - 1] == 1) return false;
        // 只有一位或0前面是0，必定为单位编码
        if (bits.length == 1 || bits[bits.length - 2] == 0) return true;

		// 统计0前面的1的数目
        int count = 0;
        for (int i = n - 2; i >= 0 && bits[i] == 1; i--) {
            count++;
        }
        return count % 2 == 0;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.4MB，在所有java提交中击败了20.00%的用户。

## 官方解题

&emsp;官方实现更加简洁。

```java
class Solution {
    public boolean isOneBitCharacter(int[] bits) {
        int i = bits.length - 2;
        while (i >= 0 && bits[i] > 0) i--;
        return (bits.length - i) % 2 == 0;
    }
}
```