[toc]

Alice and Bob take turns playing a game, with **Alice starting first**.

There are $n$ stones arranged in a row. On each player's turn, they can **remove** either the leftmost stone or the rightmost stone from the row and receive points equal to the **sum** of the remaining stones' values in the row. The winner is the one with the higher score when there are no stones left to remove.

Bob found that he will always lose this game (poor Bob, he always loses), so he decided to **minimize the score's difference**. Alice's goal is to **maximize the difference** in the score.

Given an array of integers `stones` where `stones[i]` represents the value of the `ith` stone **from the left**, return the **difference** in Alice and Bob's score if they both play **optimally**.

 

**Example 1**:

```
Input: stones = [5,3,1,4,2]
Output: 6
Explanation: 

- Alice removes 2 and gets 5 + 3 + 1 + 4 = 13 points. Alice = 13, Bob = 0, stones = [5,3,1,4].
- Bob removes 5 and gets 3 + 1 + 4 = 8 points. Alice = 13, Bob = 8, stones = [3,1,4].
- Alice removes 3 and gets 1 + 4 = 5 points. Alice = 18, Bob = 8, stones = [1,4].
- Bob removes 1 and gets 4 points. Alice = 18, Bob = 12, stones = [4].
- Alice removes 4 and gets 0 points. Alice = 18, Bob = 12, stones = [].
  The score difference is 18 - 12 = 6.
```

**Example 2**:

```
Input: stones = [7,90,5,1,100,10,10,2]
Output: 122
```



**Constraints**:

* $n == \text{stones.length}$
* $2 \le n \le 1000$
* $1 \le \text{stones[i]} \le 1000$



## 题目解读

&emsp;先手尽可能拉开分差，后手尽可能拉近分差，每次只能移除数组左边或右边的石子，分数为剩余石子的和，求先手最小分差。

```java
class Solution {
    public int stoneGameVII(int[] stones) {

    }
}
```

## 程序设计

* 动态规划数组`dp(i,j)`表示先手在数组$i \sim j$上可获得的最小分差；由于每个人都是最优操作，则先手努力扩大分差，后手努力缩小分差，这是个博弈过程，换个角度`dp(i,j)`表示在最优操作下先手可获得的最大分差；
* 若先手先择左端石子，则换为后手先择最大分差，即`dp(i,j)=leftScore - dp(i+1，j)`，选右侧同理；由于先手要扩大分差，则选择两者中最大值即可。

```java
class Solution {
    public int stoneGameVII(int[] stones) {
        int n = stones.length;
        // 表示i~j区间先手的最小差值
        int[][] min = new int[n][n];
        int[] preSum = new int[n + 1];
        for (int i = 0; i < n; i++) preSum[i + 1] = preSum[i] + stones[i];
        
        for (int len = 2; len <= n; len++) {
            for (int left = 0, right = left + len - 1; right < n; left++, right++) {
                // 选择左端和选择右端
                int leftScore = preSum[right + 1] - preSum[left + 1], rightScore = preSum[right] - preSum[left];
                // 先手最大化分差
                min[left][right] = Math.max(leftScore - min[left + 1][right], rightScore - min[left][right - 1]);
            }
        }
        
        return min[0][n - 1];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：46 ms, 在所有 Java 提交中击败了82.53%的用户。

内存消耗：46.3 MB, 在所有 Java 提交中击败了89.45%的用户。

## 官方解题

&emsp;暂无，密切关注。
