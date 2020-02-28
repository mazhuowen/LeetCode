[toc]

On an `N x N` grid, each square `grid[i][j]` represents the elevation at that point $(i,j)$.

Now rain starts to fall. At time $t$, the depth of the water everywhere is $t$. You can swim from a square to another 4-directionally adjacent square if and only if the elevation of both squares individually are at most $t$. You can swim infinite distance in zero time. Of course, you must stay within the boundaries of the grid during your swim.

You start at the top left square $(0, 0)$. What is the least time until you can reach the bottom right square $(N-1, N-1)$?



**Note:**

* $2 \le N \le 50$.
* `grid[i][j]` is a permutation of $[0, ..., N*N - 1]$.



## 题目解读

&emsp;给定矩阵，每个位置数值代表水平高度，时刻t泳池中任意位置水位为t，求从左上角到左下角的最短时间。题目提示可以在瞬间移动无限距离。

```java
class Solution {
    public int swimInWater(int[][] grid) {

    }
}
```

## 程序设计

* 由于在瞬间可以移动无限距离，意味着水平面超过的连续区域都是可达的。可以使用最小堆维护当前可达区域的不可达边界，初始为左上角方格，当水平超过最小水平面方格后，该方格变为可达，方格出堆，扩展该方格的邻接方格进入堆。重复操作直到右下角出队，结束循环。每一次循环需要涨水位。

```java
class Solution {
    public int swimInWater(int[][] grid) {
        int n;
        if((n = grid.length) <= 1) {
            return 0;
        }
        // 记录是否加入边界
        boolean[][] visit = new boolean[n][n];
        // 最小堆，记录访问过的方格的边界
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> a[2] - b[2]);
        // 初始起点
        queue.add(new int[]{0, 0, grid[0][0]});
        // 表示已加入边界
        visit[0][0] = true;
        // 初始水位
        int t = 0;
        while(!queue.isEmpty()) {
            // 水位比当前堆顶高，表示可以达到当前位置，则出队加入边界
            while(!queue.isEmpty() && t >= queue.peek()[2]) {
                int[] cur = queue.poll();
                int x = cur[0], y = cur[1];
                // 右下角可达，返回
                if(x == n - 1 && y == n - 1) {
                    return t;
                }
                // 坐标有效且未加入边界，则加入
                if(x + 1 < n && !visit[x + 1][y]) {
                    queue.add(new int[]{x + 1, y, grid[x + 1][y]});
                    visit[x + 1][y] = true;
                }
                if(x - 1 >= 0 && !visit[x - 1][y]) {
                    queue.add(new int[]{x - 1, y, grid[x - 1][y]});
                    visit[x - 1][y] = true;
                }
                if(y + 1 < n && !visit[x][y + 1]) {
                    queue.add(new int[]{x, y + 1, grid[x][y  + 1]});
                    visit[x][y  + 1] = true;
                }
                if(y - 1 >= 0 && !visit[x][y - 1]) {
                    queue.add(new int[]{x, y - 1, grid[x][y - 1]});
                    visit[x][y - 1] = true;
                }
            }
            // 水位上涨
            t++;
        }
        return t;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2\log_2N)$，空间复杂度为$O(N^2)$。

执行用时：15ms，在所有java提交中击败了71.33%的用户。

内存消耗：41.3MB，在所有java提交中击败了35.09%的用户。

## 官方解题

&emsp;官方除了提供堆的思路，还提供了二分查找的思路。每次二分水位，然后广度搜索是否可达。

```java
class Solution {
    public int swimInWater(int[][] grid) {
        int n = grid.length;
        // 此处假设0,0位置是最低水平面，题目限定最高水平面为n*n-1
        int low = grid[0][0], high = n * n - 1;
        while(low < high) {
            int mid = low + (high - low) / 2;
            // mid水位可达，则继续向下搜索
            if(bfs(grid, mid)) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }
        return low;
    }

    private boolean bfs(int[][] grid, int elevation) {
        int n = grid.length;
        // 记录是否加入栈
        boolean[][] visit = new boolean[n][n];
        Stack<int[]> stack = new Stack<>();
        visit[0][0] = true;
        stack.push(new int[]{0, 0, grid[0][0]});
        while(!stack.isEmpty()) {
            // 不可达出栈
            while(!stack.isEmpty() && stack.peek()[2] > elevation) {
                stack.pop();
            }
            // 判断
            if(stack.isEmpty()) {
                break;
            }
            int[] cur = stack.pop();
            int x = cur[0], y = cur[1];
            // 可达直接返回
            if(x == n - 1 && y == n - 1) return true;
            // 入栈
            if(x - 1 >= 0 && !visit[x - 1][y]) {
                stack.push(new int[]{x - 1, y, grid[x - 1][y]});
                visit[x - 1][y] = true;
            }
            if(x + 1 < n && !visit[x + 1][y]) {
                stack.push(new int[]{x + 1, y, grid[x + 1][y]});
                visit[x + 1][y] = true;
            }
            if(y - 1 >= 0 && !visit[x][y - 1]) {
                stack.push(new int[]{x, y - 1, grid[x][y - 1]});
                visit[x][y - 1] = true;
            }
            if(y + 1 < n && !visit[x][y + 1]) {
                stack.push(new int[]{x, y + 1, grid[x][y + 1]});
                visit[x][y + 1] = true;
            }
        }
        return false;
    }
}
```

> 此解法假设$(0,0)$位置是最低水平面，需要和面试官确认。

&emsp;时间复杂度为$O(N^2\log_2N)$，空间复杂度为$O(N^2)$。

执行用时：10ms，在所有java提交中击败了82.52%的用户。

内存消耗：41.5MB，在所有java提交中击败了33.33%的用户。