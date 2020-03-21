[toc]

There are `n` houses in a village. We want to supply water for all the houses by building wells and laying pipes.

For each house `i`, we can either build a well inside it directly with cost `wells[i]`, or pipe in water from another well to it. The costs to lay pipes between houses are given by the array `pipes`, where each `pipes[i] = [house1, house2, cost]` represents the cost to connect `house1` and `house2` together using a pipe. Connections are bidirectional.

Find the minimum total cost to supply water to all houses.



**Constraints:**

- `1 <= n <= 10000`
- `wells.length == n`
- `0 <= wells[i] <= 10^5`
- `1 <= pipes.length <= 10000`
- `1 <= pipes[i][0], pipes[i][1] <= n`
- `0 <= pipes[i][2] <= 10^5`
- `pipes[i][0] != pipes[i][1]`



## 题目解读



```java
class Solution {
    public int minCostToSupplyWater(int n, int[] wells, int[][] pipes) {

    }
}
```

## 程序设计



## 性能分析



## 官方解题

