[toc]

We have a collection of rocks, each rock has a positive integer weight.

Each turn, we choose **any two rocks** and smash them together.  Suppose the stones have weights $x$ and $y$ with $x \le y$.  The result of this smash is:

* If $x == y$, both stones are totally destroyed;
* If $x \ne y$, the stone of weight $x$ is totally destroyed, and the stone of weight $y$ has new weight $y-x$.

At the end, there is at most 1 stone left.  Return the **smallest possible** weight of this stone (the weight is 0 if there are no stones left.)



**Note:**

* $1 \le \text{stones.length} \le 30$
* $1 \le \text{stones[i]} \le 100$



## 题目解读

&emsp;每一回合任意选择两个石头进行粉碎，直到最后只剩一块石头，求最小的值。

```java
class Solution {
    public int lastStoneWeightII(int[] stones) {
        
    }
}
```

## 程序设计

* 题目中两两选择石头粉碎，问题看起来错综复杂；换一个思路，可以将石头分为容量最相近的两堆，最后粉碎的最小值就是这两堆石头的值之差，问题转化为背包问题。

```java
class Solution {
    public int lastStoneWeightII(int[] stones) {
        if (stones == null || stones.length == 0) return 0;

        int sum = 0;
        for (int stone : stones) sum += stone;
        // 转化为装一半的最大容量
        int cap = sum / 2;
        int[] dp = new int[cap + 1];

        for (int i = 0; i < stones.length; i++) {
            int stone = stones[i];
            for (int j = cap; j >= stone; j--) {
                // 选择加入或不加入当前石头
                dp[j] = Math.max(dp[j], dp[j - stone] + stone);
            }
        }
        return Math.abs(2 * dp[cap] - sum);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了95.47%的用户。

内存消耗：37.5MB，在所有java提交中击败了14.49%的用户。

## 官方解题

&emsp;暂无，密切关注。