[toc]

You want to build a house on an empty land which reaches all buildings in the shortest amount of distance. You can only move up, down, left and right. You are given a 2D grid of values `0`, `1` or `2`, where:

* Each `0` marks an empty land which you can pass by freely.
* Each `1` marks a building which you cannot pass through.
* Each `2` marks an obstacle which you cannot pass through.



Note:
There will be at least one building. If it is not possible to build such house according to the above rules, return `-1`.



## 题目解读

&emsp;给定矩形，其中`1`表示建筑，`2`表示障碍物，`0`表示空地，选择一块空地建造建筑，使得到达各个建筑的距离之和最短，如果不存在则返回`-1`。

```java
class Solution {
    public int shortestDistance(int[][] grid) {

    }
}
```

## 程序设计

* 最基本的思路是逐个空地作为起始点，进行广度优先搜索，记录总的距离和；需要注意判断是否有建筑未到达。

```java
class Solution {
    int[] delta = new int[]{0, 1, 0, -1, 0};

    public int shortestDistance(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) return -1;
        int m = grid.length, n = grid[0].length;

        int minDis = Integer.MAX_VALUE;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] != 0) continue;
                minDis = Math.min(minDis, distance(grid, i, j));
            }
        }
        return minDis == Integer.MAX_VALUE ? -1 : minDis;
    }

    private int distance(int[][] grid, int row, int col) {
        int m = grid.length, n = grid[0].length;

        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{row, col});
        int totalDis = 0, dis = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int k = 0; k < size; k++) {
                int[] cur = queue.poll();
                // 同层结点遍历标记时，更新了队列中的后续点，需要判断跳过
                if (grid[cur[0]][cur[1]] == 3 || grid[cur[0]][cur[1]] == 4) continue;
                
                // 遇到建筑，更新距离
                if (grid[cur[0]][cur[1]] == 1) {
                    totalDis += dis;
                    // 标记为已访问
                    grid[cur[0]][cur[1]] = 4;
                    continue;
                }
                // 空地，标记为已访问
                grid[cur[0]][cur[1]] = 3;
                for (int l = 0; l < 4; l++) {
                    int x = cur[0] + delta[l], y = cur[1] + delta[l + 1];
                    // 障碍物、已遍历过的跳过
                    if (x < 0 || x >= m || y < 0 || y >= n || grid[x][y] == 2
                        || grid[x][y] == 3 || grid[x][y] == 4) continue;
                    
                    queue.add(new int[]{x, y});
                }
            }
            dis++;
        }

        // 将标记恢复，并判断有否存在建筑无法到达
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 3) grid[i][j] = 0;
                else if (grid[i][j] == 4) grid[i][j] = 1;
                // 表示存在建筑不连通，距离设置为无限大
                else if (grid[i][j] == 1) totalDis = Integer.MAX_VALUE;
            }
        }
        return totalDis;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(M^2N^2)$，空间复杂度为$O(MN)$。

执行用时：390ms，在所有java提交中击败了9.95%的用户。

内存消耗：40.2MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。参考社区思路，从空地开始遍历存在重复问题，转变思路，可以从建筑开始遍历，计算每个空地到该建筑的最短距离，最后迭代判断最小距离；其次从建筑遍历还可以优化，记录当前建筑是否可到达其它建筑，如果存在建筑无法到达，则说明空地无法到达当前建筑的同时到达其它建筑，可以提前返回。

```java
class Solution {
    int[] delta = new int[]{0, 1, 0, -1, 0};
    // 记录每个空地可达的建筑数目
    int[][] reach;
    // 记录每个空地可达建筑的距离
    int[][] distance;

    public int shortestDistance(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) return -1;
        int m = grid.length, n = grid[0].length;
        this.reach = new int[m][n];
        this.distance = new int[m][n];

        // 统计建筑数目
        int totalBuild = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) totalBuild++;
            }
        }

        // 计算每个空地可达距离和建筑数目
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // 从建筑出发，如果该建筑达到不了所有的建筑，则说明不存在
                if (grid[i][j] == 1 && distance(grid, i, j) != totalBuild) return -1;
            }
        }

        // 统计比较最短距离
        int minDis = Integer.MAX_VALUE;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // 非空地或不可达所有建筑，跳过
                if (grid[i][j] != 0 || reach[i][j] != totalBuild) continue;
                minDis = Math.min(minDis, distance[i][j]);
            }
        }
        return minDis == Integer.MAX_VALUE ? -1 : minDis;
    }

    private int distance(int[][] grid, int row, int col) {
        int m = grid.length, n = grid[0].length;
        boolean[][] visited = new boolean[m][n];

        int dis = 0, building = 0;
        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{row, col});
        
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int k = 0; k < size; k++) {
                int[] cur = queue.poll();
                int i = cur[0], j = cur[1];
                if (visited[i][j]) continue;
                // 标记为已访问
                visited[i][j] = true;
                // 如果是建筑则计数
                if (grid[i][j] == 1) {
                    building++;
                    if (building > 1) continue;
                }
                // 可达计数加一，距离更新
                reach[i][j]++;
                distance[i][j] += dis;

                for (int l = 0; l < 4; l++) {
                    int x = i + delta[l], y = j + delta[l + 1];
                    // 障碍物、已遍历过的跳过
                    if (x < 0 || x >= m || y < 0 || y >= n || grid[x][y] == 2 || visited[x][y]) continue;
                    
                    queue.add(new int[]{x, y});
                }
            }
            dis++;
        }
        return building;
    }
}
```

&emsp;时间复杂度为$O(M^2N^2)$，空间复杂度为$O(MN)$。

执行用时：4ms，在所有java提交中击败了96.86%的用户。

内存消耗：39.7MB，在所有java提交中击败了100.00%的用户。