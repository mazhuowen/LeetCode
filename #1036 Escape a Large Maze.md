[toc]

In a 1 million by 1 million grid, the coordinates of each grid square are `(x, y)` with $0 \le x, y < 10^6$.

We start at the `source` square and want to reach the `target` square.  Each move, we can walk to a 4-directionally adjacent square in the grid that isn't in the given list of `blocked` squares.

Return `true` if and only if it is possible to reach the target square through a sequence of moves.



Note:

* $0 \le \text{blocked.length} \le 200$
* $\text{blocked[i].length} == 2$
* $0 \le \text{blocked[i][j]} < 10^6$
* $\text{source.length} == \text{target.length} == 2$
* $0 \le \text{source[i][j], target[i][j]} < 10^6$
* $\text{source} \ne \text{target}$



## 题目解读

&emsp;给定非常大的迷宫中的障碍物，判断是否可从起始点到达目标点。

```java
class Solution {
    public boolean isEscapePossible(int[][] blocked, int[] source, int[] target) {

    }
}
```

## 程序设计

* 

```java

```

## 性能分析

&emsp;



## 官方解题

&emsp;