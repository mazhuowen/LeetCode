[toc]

Given a non-empty 2D array grid of 0's and 1's, an island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

Count the number of distinct islands. An island is considered to be the same as another if they have the same shape, or have the same shape after rotation (90, 180, or 270 degrees only) or reflection (left/right direction or up/down direction).



**Note:** The length of each dimension in the given `grid` does not exceed 50.



## 题目解读

&emsp;与[#694 Number of Distinct Islands](./#694 Number of Distinct Islands.md)不同，需要考虑岛屿翻转等情况。

```java
class Solution {
    public int numDistinctIslands2(int[][] grid) {

    }
}
```

## 程序设计

* 在本题中，由于需要考虑旋转、翻转的情况，如果采用深度遍历路径作为岛屿唯一标识，首先遍历的起点不同，路径不同，其次即使起点相同，岛屿方向不同，路径表示不同；故不能采用路径表示，采用相对坐标表示。
* 遍历时记录坐标，然后生成旋转、翻转的坐标，减去最小坐标值得到相对坐标，然后排序编码，选择众多形状中字典序最大的表示。

```java
class Solution {
    int[] delta = new int[]{0, 1, 0, -1, 0};
    int[] beta = new int[]{1, 1, -1, -1, 1};

    int row, col;
    Set<String> set;

    public int numDistinctIslands2(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) return 0;

        row = grid.length;
        col = grid[0].length;
        set = new HashSet<>();

        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (grid[i][j] == 0) continue;

                // 深度优先遍历记录坐标
                List<int[]> shape = new ArrayList<>();
                numDistinctIslands2(grid, i, j, shape);

                set.add(productShape(shape));
            }
        }
        return set.size();
    }

    private void numDistinctIslands2(int[][] grid, int i, int j, List<int[]> shape) {
        if (i < 0 || i >= row || j < 0 || j >= col || grid[i][j] == 0) return;

        grid[i][j] = 0;
        // 加入
        shape.add(new int[]{i, j});

        for (int k = 0; k < 4; k++) {
            numDistinctIslands2(grid, i + delta[k], j + delta[k + 1], shape);
        }
    }

    // 产生旋转、翻转形状
    private String productShape(List<int[]> shape) {
            String res = null;

            int[] x = new int[shape.size()];
            int[] y = new int[shape.size()];
            int[] encode = new int[shape.size()];

            // 翻转（对x,y做正负乘法）
            for (int i = 0; i < 4; i++) {
                // 生成坐标并记录最小值
                int minX = shape.get(0)[0] * beta[i], minY = shape.get(0)[1] * beta[i + 1];
                for (int j = 0; j < shape.size(); j++) {
                    x[j] = shape.get(j)[0] * beta[i];
                    y[j] = shape.get(j)[1] * beta[i + 1];
                    minX = Math.min(minX, x[j]);
                    minY = Math.min(minY, y[j]);
                }

                // 将坐标标准化，与原点对齐，并编码
                for (int j = 0; j < x.length; j++) {
                    // 由于矩形形状长宽不等，乘两者之和而不是宽
                    encode[j] = (x[j] - minX) * (row + col) + (y[j] - minY);
                }
                // 排序
                Arrays.sort(encode);
                // 转为字符串，选择字典序最大的作为当前结构的代表
                String str = Arrays.toString(encode);
                res = res == null || res.compareTo(str) < 0 ? str : res;
            }

            // 旋转操作，即对y，x做正负乘法操作
            for (int i = 0; i < 4; i++) {
                int minX = shape.get(0)[1] * beta[i], minY = shape.get(0)[0] * beta[i + 1];
                for (int j = 0; j < shape.size(); j++) {
                    x[j] = shape.get(j)[1] * beta[i];
                    y[j] = shape.get(j)[0] * beta[i + 1];
                    minX = Math.min(minX, x[j]);
                    minY = Math.min(minY, y[j]);
                }

                for (int j = 0; j < x.length; j++) {
                    encode[j] = (x[j] - minX) * (row + col) + (y[j] - minY);
                }
                Arrays.sort(encode);
                String str = Arrays.toString(encode);
                res = res == null || res.compareTo(str) < 0 ? str : res;
            }

            return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN\log_2MN)$，空间复杂度为$O(MN)$。

执行用时：45ms，在所有java提交中击败了97.67%的用户。

内存消耗：40.1MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;上述思路参考官方。