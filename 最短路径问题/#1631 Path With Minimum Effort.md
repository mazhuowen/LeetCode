[toc]

You are a hiker preparing for an upcoming hike. You are given `heights`, a 2D array of size `rows x columns`, where `heights[row][col]` represents the height of cell `(row, col)`. You are situated in the top-left cell, `(0, 0)`, and you hope to travel to the bottom-right cell, `(rows-1, columns-1)` (i.e., **0-indexed**). You can move **up**, **down**, **left**, or **right**, and you wish to find a route that requires the minimum **effort**.

A route's **effort** is the **maximum absolute difference** in heights between two consecutive cells of the route.

Return the minimum **effort** required to travel from the top-left cell to the bottom-right cell.

 

**Example 1**:

<img src="..\images\#1631_exp1.png" style="zoom:80%;" />

```
Input: heights = [[1,2,2],[3,8,2],[5,3,5]]
Output: 2
Explanation: The route of [1,3,5,3,5] has a maximum absolute difference of 2 in consecutive cells.
This is better than the route of [1,2,2,2,5], where the maximum absolute difference is 3.
```

**Example 2**:

<img src="..\images\#1631_exp2.png" style="zoom:80%;" />

```
Input: heights = [[1,2,3],[3,8,4],[5,3,5]]
Output: 1
Explanation: The route of [1,2,3,4,5] has a maximum absolute difference of 1 in consecutive cells, which is better than route [1,3,5,3,5].
```

**Example 3**:

<img src="..\images\#1631_exp3.png" style="zoom: 67%;" />

```
Input: heights = [[1,2,1,1,1],[1,2,1,2,1],[1,2,1,2,1],[1,2,1,2,1],[1,1,1,2,1]]
Output: 0
Explanation: This route does not require any effort.
```



**Constraints**:

* $\text{rows} == \text{heights.length}$
* $\text{columns} == \text{heights[i].length}$
* $1 \le \text{rows, columns} \le 100$
* $1 \le \text{heights[i][j]} \le 10^6$



## 题目解读

&emsp;定义体力值是路径上相邻格子间高度差绝对值的最大值。求从左上到右下的最小体力消耗值。

```java
class Solution {
    public int minimumEffortPath(int[][] heights) {

    }
}
```

## 程序设计

* 由于格子可以向四个方向移动，故动态规划不再适用；粗看似乎是负权重有向无环图，更新路径上的最小权重直到无法更新，权重为两个格子间的绝对值，代码如下：

```java
class Solution {
    private static final int[] delta = new int[]{-1, 0, 1, 0, -1};

    public int minimumEffortPath(int[][] heights) {
        int r = heights.length, c = heights[0].length;
        int[][] record = new int[r][c];
        for (int[] array : record) {
            Arrays.fill(array, Integer.MAX_VALUE);
        }

        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{0, 0, 0});
        record[0][0] = 0;

        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            int x = cur[0], y = cur[1], effort = cur[2];
            for (int i = 0; i < 4; i++) {
                int newX = x + delta[i], newY = y + delta[i + 1];
                if (newX < 0 || newX >= r || newY < 0 || newY >= c) continue;
                int newEffort =  Math.max(effort, Math.abs(heights[newX][newY] - heights[x][y]));
                if (newEffort >= record[newX][newY]) continue;
                // 找到更小的代价，继续入队更新
                record[newX][newY] = newEffort;
                queue.add(new int[]{newX, newY, newEffort});
            }
        }

        return record[r - 1][c - 1];
    }
}
```

* 上述思路可以得到结果，但时间性能较差；仔细分析，由于权重是正数，实际题目就是有向加权图最短路径，使用`Dijkstra`即可：

```java
class Solution {
    private static final int[] delta = new int[]{-1, 0, 1, 0, -1};

    public int minimumEffortPath(int[][] heights) {
        int r = heights.length, c = heights[0].length;
        // 是否加入路径
        boolean[] known = new boolean[r * c];
        // 距离
        int[] dis = new int[r * c];
        Arrays.fill(dis, Integer.MAX_VALUE);
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> a[2] - b[2]);
        queue.add(new int[]{0, 0, 0});
        dis[0] = 0;
        
        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            if (known[cur[0] * c + cur[1]]) continue;
            // 找到最短边，加入路径
            known[cur[0] * c + cur[1]] = true;

            for (int i = 0; i < 4; i++) {
                int newX = cur[0] + delta[i], newY = cur[1] + delta[i + 1];
                if (newX < 0 || newX >= r || newY < 0 || newY >= c || known[newX * c + newY]) continue;
                int newEffort = Math.max(cur[2], Math.abs(heights[newX][newY] - heights[cur[0]][cur[1]]));
                if (newEffort >= dis[newX * c + newY]) continue;
                // 扩展
                dis[newX * c + newY] = newEffort;
                queue.add(new int[]{newX, newY, newEffort});
            }
        }
        return dis[dis.length - 1];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(rc\log_2rc)$，空间复杂度为$O(rc)$。

执行用时：65 ms, 在所有 Java 提交中击败了91.45%的用户

内存消耗：39.1 MB, 在所有 Java 提交中击败了51.12%的用户。

## 官方解题

&emsp;暂无，密切关注。社区还有使用二分查找假设最小代价，然后使用广度优先搜索验证的思路；如此之外，还有使用不相交集，首先将边排序，从代价最小的地方加入不相交集，直到起始点与结束点相连，此时的代价就是最小权重（之前加入不相交集的，实际上并非路径上的点，由于代价肯定比最终的路径代价大，故不造成影响）。