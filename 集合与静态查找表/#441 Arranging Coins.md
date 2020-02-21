[toc]

You have a total of n coins that you want to form in a staircase shape, where every k-th row must have exactly k coins.

Given n, find the total number of **full** staircase rows that can be formed.

n is a non-negative integer and fits within the range of a 32-bit signed integer.



## 题目解读

&emsp;给定$n$个硬币将其排列成阶梯状，即第$k$行有$k$个硬币，给出能得到的阶梯数。

```java
class Solution {
    public int arrangeCoins(int n) {
        
    }
}
```

## 程序设计

* 对于$k$层阶梯，总共有$(1 + k)k/2$个硬币，题目化解为使用$k$逼近$n$的问题。

```java
class Solution {
    public int arrangeCoins(int n) {
        if(n == 0) {
            return 0;
        }
        int left = 1, right = n;
        while(left <= right) {
            int mid = left + (right - left) / 2;
            // 避免溢出
            long res = (1 + (long)mid) * mid / 2;
            // left保持res<=n的上界，最终left为res>n，其前就是正确答案
            if(res <= n) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return right;
    }
}
```

* 可以直接利用数学只是求解，即$k = \sqrt{2*n + 0.25} - 0.5$。

```java
class Solution {
    public int arrangeCoins(int n) {
        return (int)(Math.sqrt(2 * (double)n + 0.25) - 0.5);
    }
}
```

## 性能分析

&emsp;二分查找的方法时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了88.88%的用户。

内存消耗：37MB，在所有java提交中击败了5.12%的用户。

&emsp;利用数学知识：

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：37MB，在所有java提交中击败了5.12%的用户。

## 官方解题

&emsp;暂无，密切关注。