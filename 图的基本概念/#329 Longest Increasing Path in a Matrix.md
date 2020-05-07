[toc]

Given an integer matrix, find the length of the longest increasing path.

From each cell, you can either move to four directions: left, right, up or down. You may NOT move diagonally or move outside of the boundary (i.e. wrap-around is not allowed).



## 题目解读

&emsp;查找矩形中最长递增序列。

```java
class Solution {
    public int longestIncreasingPath(int[][] matrix) {

    }
}
```

## 程序设计

* 最简单的思路是以每个点为起始点，深度优先搜索更新最大距离。最坏情况为$\begin{matrix}1 & 2 & \dots & n\\ 2 & 3 & \dots & n + 1\\\vdots & \vdots & \ddots & \vdots\\m & m + 1 & \dots &  n + m - 1\end{matrix}$，每一次都有两个选择，时间复杂度为$O(2^{M + N})$，会超时。最坏空间复杂度为递归深度$O(MN)$。

```java
class Solution {
    int maxLen;
    int[] delta = new int[]{0, 1, 0, -1, 0};

    public int longestIncreasingPath(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return 0;

        int m = matrix.length, n = matrix[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                increasingPath(matrix, i, j, 1);
            }
        }
        return maxLen;
    }

    private void increasingPath(int[][] matrix, int i, int j, int len) {
        boolean complete = true;
        for (int k = 0; k < 4; k++) {
            int x = i + delta[k], y = j + delta[k + 1];
            if (x < 0 || x >= matrix.length || y < 0
                    || y >= matrix[0].length || matrix[i][j] >= matrix[x][y]) continue;

            complete = false;
            increasingPath(matrix, x, y, len + 1);
        }
        if (complete) maxLen = Math.max(maxLen, len);
    }
}
```

* 上述存在重复遍历，可引入额外空间记录，避免重复遍历。

```java
class Solution {
    int[] delta = new int[]{0, 1, 0, -1, 0};
    int[][] record;

    public int longestIncreasingPath(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return 0;

        int m = matrix.length, n = matrix[0].length;
        int maxLen = 0;
        // 记录计算结果
        record = new int[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                maxLen = Math.max(maxLen, increasingPath(matrix, i, j));
            }
        }
        return maxLen;
    }

    private int increasingPath(int[][] matrix, int i, int j) {
        if (record[i][j] > 0) return record[i][j];

        boolean complete = true;
        for (int k = 0; k < 4; k++) {
            int x = i + delta[k], y = j + delta[k + 1];
            if (x < 0 || x >= matrix.length || y < 0
                    || y >= matrix[0].length || matrix[i][j] >= matrix[x][y]) continue;

            complete = false;
            // 记录最长路径
            record[i][j] = Math.max(record[i][j], increasingPath(matrix, x, y) + 1);
        }
        if (complete) record[i][j] = 1;
        return record[i][j];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时 :11 ms, 在所有 Java 提交中击败了76.36%的用户。

内存消耗 :39.8 MB, 在所有 Java 提交中击败了42.86%的用户。

## 官方解题

&emsp;官方还提供了拓扑排序的思路。小的值指向大的值可看做是有向边。

```java
class Solution {
    int[] delta = new int[]{0, 1, 0, -1, 0};

    public int longestIncreasingPath(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return 0;

        int m = matrix.length, n = matrix[0].length;
        // 统计入度（小的值指向大的值，可视作边）
        int[] inDegree = new int[m * n];
        for (int i = 0; i < m; i ++) {
            for (int j = 0; j < n; j++) {
                int idx = i * n + j;
                for (int k = 0; k < 4; k++) {
                    int x = i + delta[k], y = j + delta[k + 1];
                    if (x < 0 || x >= m || y < 0 || y >= n) continue;
                    if (matrix[x][y] < matrix[i][j]) inDegree[idx]++;
                }
            }
        }

        // 层次遍历，统计最大层数
        int len = 0;
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < inDegree.length; i++) {
            if (inDegree[i] == 0) queue.add(i);
        }

        while (!queue.isEmpty()) {
            len++;
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int idx = queue.poll();
                for (int k = 0; k < 4; k++) {
                    int x = idx / n + delta[k], y = idx % n + delta[k + 1];
                    if (x < 0 || x >= m || y < 0 || y >= n) continue;
                    // 入度为0，入队
                    if (matrix[x][y] > matrix[idx / n][idx % n] && --inDegree[x * n + y] == 0) queue.add(x * n + y);
                }
            }
        }
        // 返回最大层数
        return len;
    }
}
```

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：23ms，在所有java提交中击败了20.85%的用户。

内存消耗：40.3MB，在所有java提交中击败了42.86%的用户。