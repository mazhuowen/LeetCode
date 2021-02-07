[toc]

There are several cards **arranged in a row**, and each card has an associated number of points The points are given in the integer array `cardPoints`.

In one step, you can take one card from the beginning or from the end of the row. You have to take exactly $k$ cards.

Your score is the sum of the points of the cards you have taken.

Given the integer array `cardPoints` and the integer $k$, return the maximum score you can obtain.

 

**Example 1**:

```
Input: cardPoints = [1,2,3,4,5,6,1], k = 3
Output: 12
Explanation: After the first step, your score will always be 1. However, choosing the rightmost card first will maximize your total score. The optimal strategy is to take the three cards on the right, giving a final score of 1 + 6 + 5 = 12.
```

**Example 2**:

```
Input: cardPoints = [2,2,2], k = 2
Output: 4
Explanation: Regardless of which two cards you take, your score will always be 4.
```

**Example 3**:

```
Input: cardPoints = [9,7,7,9,7,7,9], k = 7
Output: 55
Explanation: You have to take all the cards. Your score is the sum of points of all cards.
```

**Example 4**:

```
Input: cardPoints = [1,1000,1], k = 1
Output: 1
Explanation: You cannot take the card in the middle. Your best score is 1. 
```

**Example 5**:

```
Input: cardPoints = [1,79,80,1,1,1,200,1], k = 3
Output: 202
```



**Constraints**:

* $1 \le \text{cardPoints.length} \le 10^5$
* $1 \le \text{cardPoints[i]} \le 10^4$
* $1 \le k \le \text{cardPoints.length}$



## 题目解读

&emsp;给定牌，每次只能拿首尾的卡牌，求可得的和的最大值。

```java
class Solution {
    public int maxScore(int[] cardPoints, int k) {

    }
}
```

## 程序设计

* 粗看似乎是使用记忆话回溯，实际上可转化为滑动窗口的最小值问题，因为选择的值只能是两端，中间是连续的未连接值。

```java
class Solution {
    public int maxScore(int[] cardPoints, int k) {
        if (cardPoints.length < k) throw new IllegalArgumentException();
        // 窗口和和指针
        int cur = 0, left = 0, right = 0;
        // 初始化窗口
        for (; right < cardPoints.length - k; right++) {
            cur += cardPoints[right];
        }

        // 数组和及窗口最小值
        int sum = cur, min = cur;
        while (right < cardPoints.length) {
            sum += cardPoints[right];
            cur += cardPoints[right++] - cardPoints[left++];
            min = Math.min(min, cur);
        }

        return sum - min;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：2 ms, 在所有 Java 提交中击败了94.88%的用户。

内存消耗：47.6 MB, 在所有 Java 提交中击败了70.84%的用户。

## 官方解题

&emsp;同上。
