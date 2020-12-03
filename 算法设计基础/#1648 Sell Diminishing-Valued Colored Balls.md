[toc]

You have an `inventory` of different colored balls, and there is a customer that wants `orders` balls of **any** color.

The customer weirdly values the colored balls. Each colored ball's value is the number of balls **of that color** you currently have in your `inventory`. For example, if you own $6$ yellow balls, the customer would pay 6 for the first yellow ball. After the transaction, there are only $5$ yellow balls left, so the next yellow ball is then valued at $5$ (i.e., the value of the balls decreases as you sell more to the customer).

You are given an integer array, `inventory`, where `inventory[i]` represents the number of balls of the `ith` color that you initially own. You are also given an integer `orders`, which represents the total number of balls that the customer wants. You can sell the balls **in any order**.

Return the **maximum** total value that you can attain after selling `orders` colored balls. As the answer may be too large, return it **modulo** $10^9 + 7$.

 

**Example 1**:

<img src="..\images\#1648_exp1.gif"  />

```
Input: inventory = [2,5], orders = 4
Output: 14
Explanation: Sell the 1st color 1 time (2) and the 2nd color 3 times (5 + 4 + 3).
The maximum total value is 2 + 5 + 4 + 3 = 14.
```

**Example 2**:

```
Input: inventory = [3,5], orders = 6
Output: 19
Explanation: Sell the 1st color 2 times (3 + 2) and the 2nd color 4 times (5 + 4 + 3 + 2).
The maximum total value is 3 + 2 + 5 + 4 + 3 + 2 = 19.
```

**Example 3**:

```
Input: inventory = [2,8,4,10,6], orders = 20
Output: 110
```

**Example 4**:

```
Input: inventory = [1000000000], orders = 1000000000
Output: 21
Explanation: Sell the 1st color 1000000000 times for a total value of 500000000500000000. 500000000500000000 modulo 109 + 7 = 21.
```



**Constraints**:

* $1 \le \text{inventory.length} \le 10^5$
* $1 \le \text{inventory[i]} \le 10^9$
* $1 \le \text{orders} \le \min(\text{sum(inventory[i])}, 10^9)$



## 题目解读

&emsp;给定需要的球的数目，求可分配的最大得分。球的得分定义为当前球的剩余数量。

```java
class Solution {
    public int maxProfit(int[] inventory, int orders) {

    }
}
```

## 程序设计

* 由于需要得分最大，每次可贪婪选择得分最大的颜色的球即可；
* 问题转化为如何在题目给定的数据规模内快速选择；首先可将球的计数进行排序，如`[1,1,3,3,5]`，第一次第二次分配都是最后一个球，此时得到的数组为`[1,1,3,3,3]`，第三次分配假设还需要六个球，则可直接分配后三个球，数组变为`[1,1,1,1,1]`；如果少于六个球，假设为四个，则首先分配后三个球各一个，然后再任意分配一个，得到数组为`[1,1,2,2,1]`；
* 可见只需维护一个指针，指针到数组尾的球之间所有球的数目相等，这样就可以根据分配的数目大小分配了；假设指针到数组尾长度为$len$，之间每种颜色的球数目与指针前的球数目的差值为为$diff$，则总共可分配$diff * len$个球，如果所需数目大于这个数目，则全部分配，指针继续向前遍历；否则结束分配，假设所需数目为$target$小于这段区间的数目，则$epoch = target / len$，即$diff - epoch + 1 \sim diff$之间的层全部分配，然后$epoch$层分配$rest$个，$rest = target \mod len$。

```java
class Solution {
    private final static int MOD = 1_000_000_007;

    public int maxProfit(int[] inventory, int orders) {
        int n = inventory.length;
        Arrays.sort(inventory);
        long value = 0L;
        int idx = n - 1, curMax = inventory[idx];
        while (idx >= 0) {
            // 遍历与curMax数目一致的颜色
            while (idx >= 0 && inventory[idx] == curMax) idx--;
            // 所有颜色的球数目一致，跳出循环进一步处理
            if (idx < 0) break;

            // 分配的长度和当前分配的最小颜色数目
            int len = n - 1 - idx, curMin = inventory[idx];
            long epoch = orders / len, rest = orders % len;
            // 当前区间所有球都加入
            if (epoch > curMax - curMin) {
                epoch = curMax - curMin;
                value = (value + (curMin + 1 + curMax) * epoch * len / 2) % MOD;
                // 迭代
                orders -= epoch * len;
                curMax = curMin;
            }
            // 分配完成
            else {
                value = (value + (curMax - epoch + 1 + curMax) * epoch * len / 2 + (curMax - epoch) * rest) % MOD;
                orders = 0;
                break;
            }
        }

        // 若还未分配完，处理所有颜色一致的情况（idx=-1需特殊处理）
        if (orders > 0 && idx == -1) {
            long epoch = orders / n, rest = orders % n;
            // 不够指定的数目
            if (epoch > curMax) throw new IllegalArgumentException("invalid param");
            value = (value + (curMax - epoch + 1 + curMax) * epoch * n / 2 + (curMax - epoch) * rest) % MOD;
        }
        return (int)value;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：28 ms, 在所有 Java 提交中击败了85.93%的用户

内存消耗：53.4 MB, 在所有 Java 提交中击败了14.27%的用户。

## 官方解题

&emsp;暂无，密切关注。