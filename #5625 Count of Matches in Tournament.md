[toc]

You are given an integer $n$, the number of teams in a tournament that has strange rules:

* If the current number of teams is **even**, each team gets paired with another team. A total of $n / 2$ matches are played, and $n / 2$ teams advance to the next round.
* If the current number of teams is **odd**, one team randomly advances in the tournament, and the rest gets paired. A total of $(n - 1) / 2$ matches are played, and $(n - 1) / 2 + 1$ teams advance to the next round.

Return the number of matches played in the tournament until a winner is decided.



**Example 1**:

```
Input: n = 7
Output: 6
Explanation: Details of the tournament: 

- 1st Round: Teams = 7, Matches = 3, and 4 teams advance.
- 2nd Round: Teams = 4, Matches = 2, and 2 teams advance.
- 3rd Round: Teams = 2, Matches = 1, and 1 team is declared the winner.
  Total number of matches = 3 + 2 + 1 = 6.
```

**Example 2**:

```
Input: n = 14
Output: 13
Explanation: Details of the tournament:

- 1st Round: Teams = 14, Matches = 7, and 7 teams advance.
- 2nd Round: Teams = 7, Matches = 3, and 4 teams advance.
- 3rd Round: Teams = 4, Matches = 2, and 2 teams advance.
- 4th Round: Teams = 2, Matches = 1, and 1 team is declared the winner.
  Total number of matches = 7 + 3 + 2 + 1 = 13.
```



**Constraints**:

* $1 \le n \le 200$



## 题目解读

&emsp;给定队伍数目，求两两比赛得到冠军的总的比赛数目。

```java
class Solution {
    public int numberOfMatches(int n) {

    }
}
```

## 程序设计

* 最基本的思路就是递归。

```java
class Solution {
    public int numberOfMatches(int n) {
        if (n <= 1) return 0;
        if (n == 2) return 1;
        return n / 2 + numberOfMatches(n / 2 + n % 2);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$。



## 官方解题

&emsp;暂无，密切关注。社区采用数学规律直接计算，比赛可看作是一棵树，每个节点要么没有子节点，要么有两个子节点，边的数目为$n - 1$。

```java
class Solution {
    public int numberOfMatches(int n) {
        return n - 1;
    }
}
```

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。
