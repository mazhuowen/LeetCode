[toc]

The `Hamming distance` between two integers is the number of positions at which the corresponding bits are different.

Given two integers `x` and `y`, calculate the Hamming distance.



**Note:**
$0 \le x, y < 2^{31}$.



## 题目解读

&emsp;数字的汉明距离定义为不相同的比特位数目，求两个数字的汉明距离。

```java
class Solution {
    public int hammingDistance(int x, int y) {

    }
}
```

## 程序设计

* 异或，然后统计$1$的个数。

```java
class Solution {
    public int hammingDistance(int x, int y) {
        int xor = x ^ y;
        return getOneBits(xor);
    }
    
    private int getOneBits(int num) {
        int count = 0;
        while (num != 0) {
            count += (num & 1);
            num >>= 1;
        }
        
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.5MB，在所有java提交中击败了6.45%的用户。

## 官方解题

&emsp;除了上述思路，官方提供了程序内置包的方法：

```java
class Solution {
    public int hammingDistance(int x, int y) {
        return Integer.bitCount(x ^ y); 
    }
}
```

&emsp;官方还提供了更进阶的布赖恩·克尼根算法，当存在大量0的时候，逐位统计毫无意义，可巧妙的通过`num & (num - 1)`除去最右侧连续的$1$。

```java
class Solution {
  public int hammingDistance(int x, int y) {
    int xor = x ^ y;
    int distance = 0;
    while (xor != 0) {
      distance += 1;
      // 删除最右侧的1
      xor = xor & (xor - 1);
    }
    return distance;
  }
}
```