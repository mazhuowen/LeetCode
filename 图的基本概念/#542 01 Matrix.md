[toc]

Given a matrix consists of 0 and 1, find the distance of the nearest 0 for each cell.

The distance between two adjacent cells is 1.



Note:

* The number of elements of the given matrix will not exceed 10,000.
* There are at least one 0 in the given matrix.
* The cells are adjacent in only four directions: up, down, left and right.



## 题目解读

&emsp;给定矩形，求其中1对应最近的0的距离。

```java
class Solution {
    public int[][] updateMatrix(int[][] matrix) {
        
    }
}
```

## 程序设计

* 将与0接壤的1加入队列，进行广度优先搜索。需注意全都是1的情况，矩阵要重置为-1。

```java
class Solution {
    int[] delta = new int[]{0, 1, 0, -1, 0};

    public int[][] updateMatrix(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) throw new IllegalArgumentException("invalid param");

        int m = matrix.length, n = matrix[0].length;
        int[][] res = new int[m][n];

        int count = 0;
        // 将边缘的1加入队列
        Queue<int[]> queue = new LinkedList<>();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) continue;
                count++;
                // 初始化为-1
                res[i][j] = -1;
                for (int k = 0; k < 4; k++) {
                    int x = i + delta[k], y = j + delta[k + 1];
                    if (x < 0 || x >= m || y < 0 || y >= n || matrix[x][y] == 1) continue;
                    // 周围存在0，加入队列
                    queue.add(new int[]{i, j});
                    res[i][j] = 1;
                    break;
                }
            }
        }

        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int[] cur = queue.poll();
                for (int k = 0; k < 4; k++) {
                    int x = cur[0] + delta[k], y = cur[1] + delta[k + 1];
                    // 坐标不合法或不是1或已经遍历过
                    if (x < 0 || x >= m || y < 0 || y >= n || matrix[x][y] == 0 || res[x][y] != -1) continue;
                    res[x][y] = res[cur[0]][cur[1]] + 1;
                    queue.add(new int[]{x, y});
                }
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：14ms，在所有java提交中击败了71.44%的用户。

内存消耗：43.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;除了上述思路，官方还提供了动态规划的思路。需要两次遍历填充，从左上到右下，从右下到左上。

```java
class Solution {
    public int[][] updateMatrix(int[][] matrix) {
        // 定义dp dp[i][j] = (i,j)到0的最短路径
        int rows = matrix.length;
        int cols = matrix[0].length;
        int[][] dp = new int[rows][cols];
        // 因为给定矩阵的元素个数不超过 10000，所以填充100001可以表示上限
        for (int[] tmp : dp) {
            Arrays.fill(tmp, 10001);
        }

        // 左上到右下
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (matrix[i][j] == 0) {
                    dp[i][j] = 0;
                } else {
                    if (i > 0) {
                        dp[i][j] = Math.min(dp[i][j], dp[i - 1][j] + 1);
                    }
                    if (j > 0) {
                        dp[i][j] = Math.min(dp[i][j], dp[i][j - 1] + 1);
                    }
                }
            }
        }
        // 右下到左上
        for (int i = rows - 1; i >= 0; i--) {
            for (int j = cols - 1; j >= 0; j--) {
                if (i < rows - 1) {
                    dp[i][j] = Math.min(dp[i][j], dp[i + 1][j] + 1);
                }
                if (j < cols - 1) {
                    dp[i][j] = Math.min(dp[i][j], dp[i][j + 1] + 1);
                }
            }
        }
        return dp;
    }
}
```

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：8ms，在所有java提交中击败了92.52%的用户。

内存消耗：43MB，在所有java提交中击败了100.00%的用户。