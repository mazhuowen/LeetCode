[toc]

You are the operator of a Centennial Wheel that has **four gondolas**, and each gondola has room for **up to four people**. You have the ability to rotate the gondolas **counterclockwise**, which costs you `runningCost` dollars.

You are given an array `customers` of length $n$ where `customers[i]` is the number of new customers arriving just before the `i`th rotation (0-indexed). This means you **must rotate** the wheel $i$ times before `customers[i]` arrive. Each customer pays `boardingCost` dollars when they board on the gondola closest to the ground and will exit once that gondola reaches the ground again.

You can stop the wheel at any time, including **before serving all customers**. If you decide to stop serving customers, **all subsequent rotations are free** in order to get all the customers down safely. Note that if there are currently more than four customers waiting at the wheel, only four will board the gondola, and the rest will wait **for the next rotation**.

Return the minimum number of rotations you need to perform to maximize your profit. If there is **no scenario** where the profit is positive, return $-1$.



**Constraints**:

* $n == \text{customers.length}$
* $1 \le n \le 10^5$
* $0 \le \text{customers[i]} \le 50$
* $1 \le \text{boardingCost, runningCost} \le 100$



## 题目解读

&emsp;摩天轮有四个座舱，每个座舱一次最多可以上去四个人，每个人需要门票`boardingCost`，摩天轮运行一轮需要花费`runningCost`，求所有轮次中的最大收益。

```java
class Solution {
    public int minOperationsMaxProfit(int[] customers, int boardingCost, int runningCost) {

    }
}
```

## 程序设计

* 题目中说当座舱再次着地时，游客会离开，但是从示例中看出游客直到最后摩天轮停下时才会离开，即座舱每次最多上四个人，原先座舱里的人数叠加即可，中途无下来的人，简单模拟整个过程。

```java
class Solution {
    public int minOperationsMaxProfit(int[] customers, int boardingCost, int runningCost) {
        // 摩天轮累积人数、等待人数、轮次
        int cur = 0, wait = 0, epoch = 1;
        // 最大收益和对应轮次
        int max = 0, res = -1;
        for (; epoch <= customers.length || wait > 0; epoch++) {
            if (epoch <= customers.length) wait += customers[epoch - 1];
            if (wait >= 4) {
                cur += 4;
                wait -= 4;
            } else {
                cur += wait;
                wait = 0;
            }
            
            // 计算本轮收益
            int cost = cur * boardingCost - epoch * runningCost;
            if (cost > max) {
                max = cost;
                res = epoch;
            }
        }

        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：13ms，在所有java提交中击败了48.71%的用户。

内存消耗：48MB，在所有java提交中击败了99.48%的用户。

## 官方解题

&emsp;暂无，密切关注。