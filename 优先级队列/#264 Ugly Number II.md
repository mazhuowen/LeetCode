[toc]

Write a program to find the n-th ugly number.

Ugly numbers are positive numbers whose prime factors only include 2, 3, 5. 



**Note:** 

* `1` is typically treated as an ugly number.
* `n` **does not exceed 1690**.



## 题目解读

&emsp;给定$n$，得出第$n$个丑数。

```java
class Solution {
    public int nthUglyNumber(int n) {
        
    }
}
```

## 程序设计

* 注意到除了第一个1，其它丑数都是由2、3、5相乘得来，想到最小语言集，可以利用同样的思路：用堆来记录丑数集，初始为1；第$n$个丑数就是出队$n$次后的那个值。每次出队需要将出队值乘以2、3、5并将结果入栈。
* 首先java的优先级队列不支持去重，`2 × 3`和`3 × 2`的值相同，会导致错误，需要记录之前出队的值，用于去重；其次乘法操作会造成整形溢出，此处采用长整形避免这种情况。

```java
class Solution {
    public int nthUglyNumber(int n) {
        PriorityQueue<Long> queue = new PriorityQueue<>();
        // 1为第一个丑数
        queue.add(1L);
        // long防止整数乘法溢出
        long uglyNum = 0, pre = 0;
        while(n > 0) {
            uglyNum = queue.poll();
            // 由于存在重复，需要去重
            if(uglyNum == pre) {
                continue;
            }
            // 其它丑数都是在原来的基础上乘2、3、5
            queue.add(uglyNum * 2);
            queue.add(uglyNum * 3);
            queue.add(uglyNum * 5);
            n--;
            // 记录上一次的值，用于去重
            pre = uglyNum;
        }
        return (int)uglyNum;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N^3)$。

执行用时：63ms，在所有java提交中击败了20.37%的用户。

内存消耗：48.4MB，在所有java提交中击败了5.21%的用户。

## 官方解题

&emsp;暂无，密切关注。社区采用动态规划达到较好的性能。重点在于乘以2、3、5的操作数的指针迭代更新。

```java
class Solution {
    public int nthUglyNumber(int n) {
        // 动态规划数组
        int[] dp = new int[n];
        dp[0] = 1;
        int dp_2 = 0;
        int dp_3 = 0;
        int dp_5 = 0;
        // 动态规划生成
        for (int i = 1; i < n; i++) {
            // 乘以2、3、5的较小者
            dp[i] = Math.min(2 * dp[dp_2], Math.min(3 * dp[dp_3], 5 * dp[dp_5]));
            // 非常重要，定位下次计算位置
            if (dp[i] >= 2 * dp[dp_2]) {
                dp_2++;
            }
            if (dp[i] >= 3 * dp[dp_3]) {
                dp_3++;
            }
            if (dp[i] >= 5 * dp[dp_5]) {
                dp_5++;
            }
        }
        return dp[n - 1];
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：4ms，在所有java提交中击败了81.19%的用户。

内存消耗：40.1MB，在所有java提交中击败了5.21%的用户。