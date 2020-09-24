[toc]

桌上有 $n$ 堆力扣币，每堆的数量保存在数组 `coins` 中。我们每次可以选择任意一堆，拿走其中的一枚或者两枚，求拿完所有力扣币的最少次数。



**限制：**

- $1 \le n \le 4$
- $1 \le \text{coins[i]} \le 10$



## 题目解读

&emsp;求拿完所有力扣币的最少次数。

```java
class Solution {
    public int minCount(int[] coins) {

    }
}
```

## 程序设计

* 由于每次只能选一堆，而每堆一次可以拿走一个或两个，即每一堆最少拿走数目是固定的，不受选择顺序影响，故可单独计算每一堆的数目，而后相加；
* 每一堆采用贪婪法，每次拿走两个，如果最后还剩余一个，则最后一次拿走一个。

```java
class Solution {
    public int minCount(int[] coins) {
        int res = 0;
        for (int coin : coins) res += minCount(coin);
        return res; 
    }

    private int minCount(int coin) {
        return coin / 2 + coin % 2;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.2MB，在所有java提交中击败了58.05%的用户。

## 官方解题

&emsp;同上，对每一堆的计算优化为$(coin + 1) / 2$。