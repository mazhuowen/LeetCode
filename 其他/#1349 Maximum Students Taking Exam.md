[toc]

Given a $m * n$ matrix `seats`  that represent seats distributions in a classroom. If a seat is broken, it is denoted by `'#'` character otherwise it is denoted by a `'.'` character.

Students can see the answers of those sitting next to the left, right, upper left and upper right, but he cannot see the answers of the student sitting directly in front or behind him. Return the **maximum** number of students that can take the exam together without any cheating being possible..

Students must be placed in seats in good condition.

 

**Example 1**:

<img src="..\images\#1349_exp1.png" style="zoom:67%;" />

```
Input: seats = [["#",".","#","#",".","#"],
                [".","#","#","#","#","."],
                ["#",".","#","#",".","#"]]
Output: 4
Explanation: Teacher can place 4 students in available seats so they don't cheat on the exam. 
```

**Example 2**:

```
Input: seats = [[".","#"],
                ["#","#"],
                ["#","."],
                ["#","#"],
                [".","#"]]
Output: 3
Explanation: Place all students in available seats. 
```

**Example 3**:

```
Input: seats = [["#",".",".",".","#"],
                [".","#",".","#","."],
                [".",".","#",".","."],
                [".","#",".","#","."],
                ["#",".",".",".","#"]]
Output: 10
Explanation: Place students in available seats in column 1, 3 and 5.
```



**Constraints**:

* `seats` contains only characters `'.'` and `'#'`.
* $m == \text{seats.length}$
* $n == \text{seats[i].length}$
* $1 \le m \le 8$
* $1 \le n \le 8$



## 题目解读

&emsp;学生可以抄袭左侧、右侧、左上、右上的考卷，给定考场座位分布，求可容纳的最大学生数，使得学生无法抄袭。

```java
class Solution {
    public int maxStudents(char[][] seats) {

    }
}
```

## 程序设计

* 轮廓线动态规划，由于需要左上的值，故保存当前位置前$n+1$个点的状态。

```java
class Solution {
    int m, n, mod;
    char[][] seats;
    int[][][] dp;

    public int maxStudents(char[][] seats) {
        this.m = seats.length;
        this.n = seats[0].length;
        // 低n位取模
        this.mod = (1 << n) - 1;
        this.seats = seats;
        // 分别表示x、y、当前位置其前n+1个格子的状态
        this.dp = new int[m][n][1 << (n + 1)];

        return backTracing(0, 0, 0);
    }

    private int backTracing(int x, int y, int preStat) {
        if (x == m) return 0;
        if (y == n) return backTracing(x + 1, 0, preStat);
        if (dp[x][y][preStat] != 0) return dp[x][y][preStat];

        // 当前座位不分配
        int res = backTracing(x, y + 1, (preStat & mod) << 1);
        // 左上、右上、左侧
        int upperLeft = preStat >> n, upperRight = (preStat >> (n - 2)) & 1, left = preStat & 1;
        // 当前座位分配
        if (checkValid(x, y, upperLeft, upperRight, left)) {
            res = Math.max(res, 1 + backTracing(x, y + 1, markStat(preStat, x, y)));
        }
        
        return dp[x][y][preStat] = res;
    }

    private boolean checkValid(int x, int y, int upperLeft, int upperRight, int left) {
        // 当前座位损坏
        if (seats[x][y] == '#') return false;
        // 左侧已有人占用
        if (y > 0 && seats[x][y - 1] == '.' && left == 1) return false;
        // 左上有人占用
        if (x > 0 && y > 0 && seats[x - 1][y - 1] == '.' && upperLeft == 1) return false;
        // 右上有人占用
        if (x > 0 && y + 1 < n && seats[x - 1][y + 1] == '.' && upperRight == 1) return false;
        // 可以分配
        return true; 
    }

    private int markStat(int preStat, int x, int y) {
        // 出队，设置当前，需注意不能设置右上或左上及左侧，只能设置当前
        return (preStat & mod) << 1 | 1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN2^N)$，空间复杂度为$O(MN2^N)$。

执行用时：1 ms, 在所有 Java 提交中击败了100.00%的用户。

内存消耗：38.1 MB, 在所有 Java 提交中击败了14.45%的用户。

## 官方解题

&emsp;官方思路为状态压缩动态规划，时间复杂度较高。
