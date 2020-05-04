[toc]

Given an $m \times n$ matrix of non-negative integers representing the height of each unit cell in a continent, the "Pacific ocean" touches the left and top edges of the matrix and the "Atlantic ocean" touches the right and bottom edges.

Water can only flow in four directions (up, down, left, or right) from a cell to another one with height equal or lower.

Find the list of grid coordinates where water can flow to both the Pacific and Atlantic ocean.

Note:

* The order of returned grid coordinates does not matter.
* Both $m$ and $n$ are less than 150.



## 题目解读

&emsp;给定矩形表示陆地的海平面，上、左表示太平洋，右下表示大西洋，判断哪些点水可以流向两洋。

```java
class Solution {
    public List<List<Integer>> pacificAtlantic(int[][] matrix) {

    }
}
```

## 程序设计

* 首先从上、右边界广度优先搜索所有可以到达太平洋的点，然后从下、左广度优先搜索比较可以到达两洋的点。

```java
class Solution {
    int[] delta = new int[]{1, 0, -1, 0, 1};

    public List<List<Integer>> pacificAtlantic(int[][] matrix) {
        List<List<Integer>> res = new LinkedList<>();
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return res;

        int m = matrix.length, n = matrix[0].length;

        // 1表示可达太平洋，2表示可达大西洋
        int[][] visited = new int[m][n];
        Queue<int[]> pacific = new LinkedList<>();
        Queue<int[]> atlantic = new LinkedList<>();
        for (int i = 0; i < m; i++) {
            pacific.add(new int[]{i, 0});
            atlantic.add(new int[]{i, n - 1});
        }
        for (int j = 0; j < n; j++) {
            pacific.add(new int[]{0, j});
            atlantic.add(new int[]{m - 1, j});
        }

        // 广度优先搜索
        while (!pacific.isEmpty()) {
            int size = pacific.size();
            for (int i = 0; i < size; i++) {
                int[] cur = pacific.poll();
                visited[cur[0]][cur[1]] = 1;

                for (int j = 0; j < 4; j++) {
                    int x = cur[0] + delta[j], y = cur[1] + delta[j + 1];
                    // 坐标不符合要求，或已经标记过，或海平面不符合传播要求（从低到高传播）
                    if (x < 0 || y < 0 || x >= m || y >= n || visited[x][y] == 1 || matrix[cur[0]][cur[1]] > matrix[x][y]) continue;
                    pacific.add(new int[]{x, y});
                }
            }
        }

        while (!atlantic.isEmpty()) {
            int size = atlantic.size();
            for (int i = 0; i < size; i++) {
                int[] cur = atlantic.poll();
                // 当前点可到两洋
                if (visited[cur[0]][cur[1]] == 1) {
                    List<Integer> temp = new LinkedList<>();
                    temp.add(cur[0]);
                    temp.add(cur[1]);
                    res.add(temp);
                }
                // 标记为可达大西洋
                visited[cur[0]][cur[1]] = 2;

                for (int j = 0; j < 4; j++) {
                    int x = cur[0] + delta[j], y = cur[1] + delta[j + 1];
                    if (x < 0 || y < 0 || x >= m || y >= n || visited[x][y] == 2 || matrix[cur[0]][cur[1]] > matrix[x][y]) continue;
                    atlantic.add(new int[]{x, y});
                }
            }
        }

        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：11ms，在所有java提交中击败了24.41%的用户。

内存消耗：41.3MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区时间性能较好的方法使用深度优先搜索标记可以到达两洋的点，然后再遍历判断。