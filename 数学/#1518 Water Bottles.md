[toc]

Given `numBottles` full water bottles, you can exchange `numExchange` empty water bottles for one full water bottle.

The operation of drinking a full water bottle turns it into an empty bottle.

Return the **maximum** number of water bottles you can drink.



**Constraints:**

- $1 \le \text{numBottles} \le 100$
- $2 \le \text{numExchange} \le 100$



## 题目解读

&emsp;每`numBottles`个空瓶可换一瓶水，求最多可换的水。

```java
class Solution {
    public int numWaterBottles(int numBottles, int numExchange) {

    }
}
```

## 程序设计

* 递归：

```java
class Solution {
    public int numWaterBottles(int numBottles, int numExchange) {
        // 无法换瓶盖
        if (numBottles < numExchange) return numBottles;
        // 换取的水
        int change = numBottles / numExchange;
        // 剩余的水
        int rest = numBottles % numExchange;
        // 当前喝numBottles - rest
        return numBottles - rest + numWaterBottles(change + rest, numExchange);
    }
}
```

* 迭代：

```java
class Solution {
    public int numWaterBottles(int numBottles, int numExchange) {
        int res = 0;
        // 可交换水
        while (numBottles >= numExchange) {
            int rest = numBottles % numExchange;
            // 本轮喝水
            res += numBottles - rest;
            // 剩余的水
            numBottles = numBottles / numExchange + rest;
        }
        return res + numBottles;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N/M)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：35.8MB，在所有java提交中击败了12.53%的用户。

## 官方解题

&emsp;官方还提供数学的思路，由于每次换取瓶子都会损失`numExchange - 1`个牌子，假设可以交换$n$次，则`numBottles - n(numExchange - 1) < numExchange `就无法交换，则求出交换的次数$n$，即交换来$n$瓶水，加上原来的水就是总的可喝到的水。

```java
class Solution {
    public int numWaterBottles(int numBottles, int numExchange) {
        return numBottles >= numExchange ? (numBottles - numExchange) / (numExchange - 1) + 1 + numBottles : numBottles;
    }
}
```

