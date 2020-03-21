[toc]

We are given a matrix with `R` rows and `C` columns has cells with integer coordinates $(r, c)$, where $0 \le r < R$ and $0 \le c < C$.

Additionally, we are given a cell in that matrix with coordinates (r0, c0).

Return the coordinates of all cells in the matrix, sorted by their distance from $(r_0, c_0)$ from smallest distance to largest distance.  Here, the distance between two cells $(r_1, c_1)$ and $(r_2, c_2)$ is the Manhattan distance, $\vert r_1 - r_2\vert + \vert c_1 - c_2\vert$.  (You may return the answer in any order that satisfies this condition.)

**Note:**

* $1 \le R \le 100$
* $1 \le C \le 100$
* $0 \le r_0 < R$
* $0 \le c_0 < C$



## 题目解读

&emsp;给定一个点，输出给定矩形到该点曼哈顿距离有小到大的数组。

```java
class Solution {
    public int[][] allCellsDistOrder(int R, int C, int r0, int c0) {

    }
}
```

## 程序设计

* 最基本的想法是广度优先遍历。

```java
class Solution {
    public int[][] allCellsDistOrder(int R, int C, int r0, int c0) {
        if(R <= 0 || C <= 0 || r0 < 0 || c0 < 0 || c0 >= C || r0 >= R) {
            return new int[][]{};
        }
        // 记录是否访问过
        boolean[][] visit = new boolean[R][C];
        int[][] res = new int[R * C][2];
        int idx = 0;
        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{r0, c0});
        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            if(cur[0] >= R || cur[0] < 0 || cur[1] >= C || cur[1] < 0 || visit[cur[0]][cur[1]]) {
                continue;
            }
            res[idx++] = cur;
            visit[cur[0]][cur[1]] = true;
            queue.add(new int[]{cur[0] + 1, cur[1]});
            queue.add(new int[]{cur[0] - 1, cur[1]});
            queue.add(new int[]{cur[0], cur[1] + 1});
            queue.add(new int[]{cur[0], cur[1] - 1});
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(RC)$，空间复杂度为$O(RC)$。

执行用时：21ms，在所有java提交中击败了34.99%的用户。

内存消耗：43.2MB，在所有java提交中击败了5.12%的用户。

## 官方解题

&emsp;暂无，密切关注。