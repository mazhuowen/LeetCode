[toc]

A group of two or more people wants to meet and minimize the total travel distance. You are given a 2D grid of values 0 or 1, where each 1 marks the home of someone in the group. The distance is calculated using `Manhattan Distance`, where distance$(p_1, p_2) = \vert p_2.x - p_1.x\vert + \vert p_2.y - p_1.y\vert$.



## 题目解读

&emsp;给定矩阵，1表示人所在的位置，求矩阵里的人会面需要的最短距离。距离用曼哈顿距离计算。

```java
class Solution {
    public int minTotalDistance(int[][] grid) {

    }
}
```

## 程序设计

* 首先想到的是遍历矩阵每个点，然后以这个点为会面点，遍历周围的点，并计算距离。最后选择最小的那个点。但是这个方法会超时。

```java
class Solution {
    public int minTotalDistance(int[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }
        int res = Integer.MAX_VALUE;
        // 遍历每个可能的会面点，然后计算距离
        for (int row = 0; row < grid.length; row++) {
            for (int col = 0; col < grid[0].length; col++) {
                int dis = search(grid, row, col);
                res = Math.min(res, dis);
            }
        }
        return res;
    }
    
    private int search(int[][] grid, int row, int col) {
        int dis = 0;
        boolean[][] visit = new boolean[grid.length][grid[0].length];
        Queue<int[]> queue = new LinkedList<>();
        // 初始化当前点到会面点距离为0
        queue.add(new int[]{row, col, 0});
        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            int r  =cur[0];
            int c = cur[1];
            int d = cur[2];
            if (r < 0 || r >= grid.length || c < 0 || c >= grid[0].length || visit[r][c]) {
                continue;
            }
            visit[r][c] = true;
            // 当前点是人，更新距离
            if (grid[r][c] == 1) {
                dis += d;
            }
            // 由于是曼哈顿距离，前后左右距离加一
            queue.add(new int[]{r + 1, c, d + 1});
            queue.add(new int[]{r - 1, c, d + 1});
            queue.add(new int[]{r, c + 1, d + 1});
            queue.add(new int[]{r, c - 1, d + 1});
        }
        return dis;
    }
}
```

* 由于曼哈顿距离只需终点和起点，无需中间过程，可以遍历是1的点直接计算。

```java
class Solution {
    public int minTotalDistance(int[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }
        int res = Integer.MAX_VALUE;
        // 遍历每个可能的会面点，然后计算距离
        for (int row = 0; row < grid.length; row++) {
            for (int col = 0; col < grid[0].length; col++) {
                int dis = calcuDistance(grid, row, col);
                res = Math.min(res, dis);
            }
        }
        return res;
    }
	// 直接遍历计算
    private int calcuDistance(int[][] grid, int row, int col) {
        int dis = 0;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == 1) {
                    dis += Math.abs(i - row) + Math.abs(j - col);
                }
            }
        }
        return dis;
    }
}
```

* 考虑到曼哈顿距离`x`轴和`y`轴是分开计算的，假设只考虑一维情况：`0-1-0-1-0-1-1`，可以观察到左右各有两个点，这时最短会面点在左右两侧中间，也就是索引3、4、5的三个点都是最短会面点，最短距离为7，这样可以总结出会面者为偶数个的情况，最佳会面点在中间任意点；如果会面者是奇数，比如：`0-1-0-0-0-1-1`，如果不考虑中间为奇数的点，也就是索引为5的点，则最佳距离在索引1~6之间的任一点，现在考虑索引为5的点，若会面点在5则到5的距离仍然是0，总的距离不变，若不是5则总的距离在原来基础上还要增加，即奇数个点最佳会面点就是中间的点。
* 如果结合上面两种情况，奇数个点是中间点，偶数个点是中间区域，包含中间点，则可以统一以中间点为会面点来计算距离。注意此处的中间点不是所有的点的中间点，而是会面者的中间点。这样就可以统计所有的会面者的行坐标、列坐标，分别计算`x`轴、`y`轴距离，最后综合得到结果。

```java
class Solution {
    public int minTotalDistance(int[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }
        List<Integer> rows = new ArrayList<>();
        List<Integer> cols = new ArrayList<>();
        // 统计行列坐标
        for (int row = 0; row < grid.length; row++) {
            for (int col = 0; col < grid[0].length; col++) {
                if (grid[row][col] == 1) {
                    rows.add(row);
                    cols.add(col);
                }
            }
        }
        if (rows.size() == 0) {
            return 0;
        }
        // 行坐标是有序的，无需排序
        int midRow = rows.get(rows.size() / 2);
        // 列坐标是无序的，需要根据坐标排序
        Collections.sort(cols);
        int midCol = cols.get(cols.size() / 2);
        // 选取中间点计算
        return calcuDistance(rows, midRow) + calcuDistance(cols, midCol);
    }

    private int calcuDistance(List<Integer> list, int base) {
        int res = 0;
        for(int num : list) {
            res += Math.abs(num - base);
        }
        return res;
    }
}
```

## 性能分析

&emsp;超时的方法的方法时间复杂度为$O(N^2M^2)$，空间复杂度为$O(NM)$。暴力遍历时间复杂度为$O(N^2M^2)$，空间复杂度为$O(1)$。

执行用时：339ms，在所有java提交中击败了6.33%的用户。

内存消耗：39.3MB，在所有java提交中击败了95.45%的用户。

&emsp;坐标分开计算时间复杂度为$O(NM\log_2NM)$，空间复杂度为$O(min(N,M))$。

执行用时：8ms，在所有java提交中击败了67.09%的用户。

内存消耗：41.3MB，在所有java提交中击败了45.45%的用户。

## 官方解题

&emsp;除了上述思路，官方进一步做了优化。上面为了找中间数，需要对列坐标做排序。如果可以分两次循环，分别按行坐标遍历和按列坐标遍历，则时间复杂度会继续下降。

```java
class Solution {
    public int minTotalDistance(int[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }
        // 分别按行、列计算
        List<Integer> rows = collectRows(grid);
        List<Integer> cols = collectCols(grid);
        if (rows.size() == 0) {
            return 0;
        }
        int row = rows.get(rows.size() / 2);
        int col = cols.get(cols.size() / 2);
        return minDistance1D(rows, row) + minDistance1D(cols, col);
    }

    private int minDistance1D(List<Integer> points, int origin) {
        int distance = 0;
        for (int point : points) {
            distance += Math.abs(point - origin);
        }
        return distance;
    }
	// 按行遍历
    private List<Integer> collectRows(int[][] grid) {
        List<Integer> rows = new ArrayList<>();
        for (int row = 0; row < grid.length; row++) {
            for (int col = 0; col < grid[0].length; col++) {
                if (grid[row][col] == 1) {
                    rows.add(row);
                }
            }
        }
        return rows;
    }
	// 按列遍历
    private List<Integer> collectCols(int[][] grid) {
        List<Integer> cols = new ArrayList<>();
        for (int col = 0; col < grid[0].length; col++) {
            for (int row = 0; row < grid.length; row++) {
                if (grid[row][col] == 1) {
                    cols.add(col);
                }
            }
        }
        return cols;
    }
}
```

另外可以不直接用中间数计算，使用双指针计算也是一样的。

```java
class Solution {
    public int minTotalDistance(int[][] grid) {
        if(grid == null || grid.length == 0) {
            return 0;
        }
        List<Integer> rows = collectRows(grid);
        List<Integer> cols = collectCols(grid);
        if (rows.size() == 0) {
            return 0;
        }
        return minDistance1D(rows) + minDistance1D(cols);
    }

    private int minDistance1D(List<Integer> points) {
        int distance = 0;
        int start = 0, end = points.size() - 1;
        // 使用双指针计算
        while (start < end) {
            distance += points.get(end--) - points.get(start++);
        }
        return distance;
    }

    private List<Integer> collectRows(int[][] grid) {
        List<Integer> rows = new ArrayList<>();
        for (int row = 0; row < grid.length; row++) {
            for (int col = 0; col < grid[0].length; col++) {
                if (grid[row][col] == 1) {
                    rows.add(row);
                }
            }
        }
        return rows;
    }

    private List<Integer> collectCols(int[][] grid) {
        List<Integer> cols = new ArrayList<>();
        for (int col = 0; col < grid[0].length; col++) {
            for (int row = 0; row < grid.length; row++) {
                if (grid[row][col] == 1) {
                    cols.add(col);
                }
            }
        }
        return cols;
    }
}
```

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(min(N,M))$。

执行用时：4ms，在所有java提交中击败了89.87%的用户。

内存消耗：40.8MB，在所有java提交中击败了50.00%的用户。