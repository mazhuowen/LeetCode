[toc]

In a given 2D binary array `A`, there are two islands.  (An island is a 4-directionally connected group of `1`s not connected to any other `1`s.)

Now, we may change `0`s to `1`s so as to connect the two islands together to form 1 island.

Return the smallest number of `0`s that must be flipped.  (It is guaranteed that the answer is at least 1.)



**Note:**

* $1 \le \text{A.length} = \text{A[0].length} \le 100$
* $\text{A[i][j]} == 0$ or $\text{A[i][j]} == 1$



## 题目解读

&emsp;给定有两个小岛构成的图，找到连通需要的最少的数目。

```java
class Solution {
    public int shortestBridge(int[][] A) {

    }
}
```

## 程序设计

* 首先遍历其中一个岛屿，标记为2，然后从该岛屿广度优先遍历，直到遇到标记为1的岛屿，返回层数即可。
* 在层次遍历时需要注意不要重复遍历，因为当前位置需要经过之前遍历过的路径，如果当前点能到另一个岛屿，必然之前那个点也能到另一个岛屿且路径更短，如果不标识已经遍历过的点，会造成超时。
* 综上，标记一个岛屿为2，从该岛屿出发向外层遍历，遍历位置标记为2，直到遇到另一个岛屿，也就是1的点，停止遍历并返回结果。

```java
class Solution {
    public int shortestBridge(int[][] A) {
        if (A == null || A.length < 2) throw new IllegalArgumentException("invalid param");

        int n = A.length;
        // 标记其中一个岛，并入队
        Queue<int[]> queue = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            boolean flag = false;
            for (int j = 0; j < n; j++) {
                if (A[i][j] == 1) {
                    dfs(i, j, A, queue);
                    // 已标记一个岛，不需要继续遍历，跳出
                    flag = true;
                    break;
                }
            }
            if (flag) break;
        }

        int[] dx = new int[]{-1, 1, 0 ,0};
        int[] dy = new int[]{0, 0, -1, 1};

        // 开始层次搜索
        int count = 0;
        while (!queue.isEmpty()) {
            // 同一层出队判断
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int[] cur = queue.poll();

                // 入队下一层
                for (int j = 0; j < 4; j++) {
                    int x = cur[0] + dx[j], y = cur[1] + dy[j];
                    // 坐标不符合要求，或者路径是已遍历过的，不予入队
                    if (x < 0 || x >= n || y < 0 || y >= n || A[x][y] == 2) continue;
                    // 到达另一个岛屿，返回
                    if (A[x][y] == 1) return count;
                    // 加入队列的标记水的区域为2
                    A[x][y] = 2;
                    queue.add(new int[]{x, y});
                }
            }
            // 层数增加
            count++;
        }
        throw new IllegalArgumentException("logic error");
    }

    private void dfs(int row, int col, int[][] A, Queue<int[]> queue) {
        if (row < 0 || row >= A.length || col < 0 || col >= A.length || A[row][col] != 1) return;
        // 入队并标记为2
        A[row][col] = 2;
        queue.add(new int[]{row, col});

        dfs(row - 1, col, A, queue);
        dfs(row + 1, col, A, queue);
        dfs(row, col - 1, A, queue);
        dfs(row, col + 1, A, queue);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：8ms，在所有java提交中击败了97.81%的用户。

内存消耗：42.5MB，在所有java提交中击败了100%的用户。

## 官方解题

&emsp;上述思路参考官方解题。