[toc]

In the world of Dota2, there are two parties: the `Radiant` and the `Dire`.

The Dota2 senate consists of senators coming from two parties. Now the senate wants to make a decision about a change in the Dota2 game. The voting for this change is a round-based procedure. In each round, each senator can exercise `one` of the two rights:

* `Ban one senator's right`:
  A senator can make another senator lose **all his rights** in this and all the following rounds.
* `Announce the victory`:
  If this senator found the senators who still have rights to vote are all from **the same party**, he can announce the victory and make the decision about the change in the game.

Given a string representing each senator's party belonging. The character `'R'` and `'D'` represent the `Radiant` party and the `Dire` party respectively. Then if there are n senators, the size of the given string will be $n$.

The round-based procedure starts from the first senator to the last senator in the given order. This procedure will last until the end of voting. All the senators who have lost their rights will be skipped during the procedure.

Suppose every senator is smart enough and will play the best strategy for his own party, you need to predict which party will finally announce the victory and make the change in the Dota2 game. The output should be `Radiant` or `Dire`.



**Example 1**:

```
Input: "RD"
Output: "Radiant"
Explanation: The first senator comes from Radiant and he can just ban the next senator's right in the round 1. 
And the second senator can't exercise any rights any more since his right has been banned. 
And in the round 2, the first senator can just announce the victory since he is the only guy in the senate who can vote.
```

**Example 2**:

```
Input: "RDD"
Output: "Dire"
Explanation: 
The first senator comes from Radiant and he can just ban the next senator's right in the round 1. 
And the second senator can't exercise any rights anymore since his right has been banned. 
And the third senator comes from Dire and he can ban the first senator's right in the round 1. 
And in the round 2, the third senator can just announce the victory since he is the only guy in the senate who can vote.
```



**Note**:

* The length of the given string will in the range $[1, 10000]$.



## 题目解读

&emsp;给定议员序列，按照顺序操作，议员分两派，议员可以让另一个议员出局，最后议员都是一派则获胜。求最后获胜的一方。

```java
class Solution {
    public String predictPartyVictory(String senate) {
        
    }
}
```

## 程序设计

* 使用贪心思路，没名议员应该优先排除其后的第一名不同派议员。采用队列来实现，每一轮结束索引增加。

```java
class Solution {
    public String predictPartyVictory(String senate) {
        Queue<Integer> radiant = new LinkedList<>(), dire = new LinkedList<>();
        int n = senate.length();
        for (int i = 0; i < n; i++) {
            char c = senate.charAt(i);
            if (c == 'R') radiant.add(i);
            else dire.add(i);
        }

        while (!radiant.isEmpty() && !dire.isEmpty()) {
            int r = radiant.poll(), d = dire.poll();
            // r在前，投票让d出局，r再次入队
            if (r < d) radiant.add(r + n);
            // d在前
            else dire.add(d + n);

        }
        return radiant.isEmpty() ? "Dire" : "Radiant";
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：12 ms, 在所有 Java 提交中击败了74.51%的用户。

内存消耗：39.3 MB, 在所有 Java 提交中击败了33.66%的用户。

## 官方解题

&emsp;官方思路见上。
