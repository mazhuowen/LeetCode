[toc]

Given a 2D `grid` of `0`s and `1`s, return the number of elements in the largest **square** subgrid that has all `1`s on its **border**, or `0` if such a subgrid doesn't exist in the `grid`.



Constraints:

* $1 \le \text{grid.length} \le 100$
* $1 \le \text{grid[0].length} \le 100$
* `grid[i][j]` is `0` or `1`



## 题目解读

&emsp;找到边界为1的最大正方形，返回正方形中的格子数。

```java
class Solution {
    public int largest1BorderedSquare(int[][] grid) {

    }
}
```

## 程序设计

* 最基本的思路是暴力遍历，如果当前点是1，则以当前点作为正方形左上顶点，先检索符合条件的上边和左边，再校验下边和右边。

```java
class Solution {
    public int largest1BorderedSquare(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) return 0;
        int m = grid.length, n = grid[0].length;

        int count = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 0) continue;

                // 当前点为一个正方形，更新
                int c = 1;
                count = Math.max(count, 1);
                // 遍历符合条件的上边和左边
                while (i + c < m && j + c < n && grid[i + c][j] == 1 && grid[i][j + c] == 1) {
                    boolean flag = true;
                    // 检查下边
                    for (int k = i + c; k > i && flag; k--) {
                        if (grid[k][j + c] == 0) flag = false;
                    }
                    // 检查右边
                    for (int k = j + c; k > j && flag; k--) {
                        if (grid[i + c][k] == 0) flag = false;
                    }
                    c++;
                    if (flag) count = Math.max(count, c);
                }
            }
        }
        return count * count;
    }
}
```

* 上述思路需要4层循环，如果要减少循环，需引入额外空间保存每个点上方和左边连续的1的点的数目，遍历当前结点作为正方形的右下顶点。

```java
class Solution {
    public int largest1BorderedSquare(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) return 0;
        int m = grid.length, n = grid[0].length;

        // 记录当前点的上方和左方连续结点数目
        int[] up = new int[m * n], left = new int[m * n];

        int count = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 0) continue;

                int idx = i * n + j;
                up[idx] = i > 0 ? up[idx - n] + 1 : 1;
                left[idx] = j > 0 ? left[idx - 1] + 1 : 1;
                // 可能的正方形的最大边长
                int max = Math.min(up[idx], left[idx]);
                // 遍历检查，遇到满足条件的第一个值就是当前点为右下顶点的最大正方形
                int tempX = i - max + 1, tempY = j - max + 1;
                while (tempX <= i) {
                    // 上边和左边满足条件
                    if (left[tempX * n + j] >= max && up[i * n + tempY] >= max) break;
                    // 不满足条件继续寻找
                    tempX++;
                    tempY++;
                    max--;
                }
                count = Math.max(count, max);
            }
        }
        return count * count;
    }
}
```

## 性能分析

&emsp;暴力遍历法时间复杂度为$O(M^2N^2)$，凯南复杂度为$O(1)$。

执行用时：6ms，在所有java提交中击败了86.89%的用户。

内存消耗：40.9MB，在所有java提交中击败了100.00%的用户。

&emsp;额外记录的方法时间复杂度为$O(MN\min(M,N))$，空间复杂度为$O(MN)$。

执行用时：6ms，在所有java提交中击败了86.89%的用户。

内存消耗：40.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。