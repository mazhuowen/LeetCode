[toc]

In a 2D grid of `0`s and `1`s, we change at most one $0$ to a $1$.

After, what is the size of the largest island? (An island is a 4-directionally connected group of $1$s).



**Notes**:

- $1 \le \text{grid.length} = \text{grid[0].length} \le 50$.
- $0 \le \text{grid[i][j]} \le 1$.



## 题目解读

&emsp;在原先的图中最多改变一个格子为陆地，求最大的岛屿面积。

```java
class Solution {
    public int largestIsland(int[][] grid) {

    }
}
```

## 程序设计

* 由于规模较少，可遍历每个非岛屿点，然后使用深度优先搜索计算最大面积，时间复杂度为$O(N^2M^2)$；
* 可使用不相交集记录岛屿，然后再次遍历判断水域连接的岛屿，并计算最大面积。

```java
class Solution {
    int[] delta = new int[]{1, 0, -1, 0, 1};

    public int largestIsland(int[][] grid) {
        int n = grid.length, m = grid[0].length;
        DisJoint disJoint = new DisJoint(n * m);
        // 遍历岛屿
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] == 0) continue;
                int idx = i * m + j;
                if (i > 0 && grid[i - 1][j] == 1) disJoint.union(disJoint.find(idx - m), disJoint.find(idx));
                if (j > 0 && grid[i][j - 1] == 1) disJoint.union(disJoint.find(idx - 1), disJoint.find(idx));
            }
        }
        // 全部都是岛屿
        if (disJoint.size == 1) return n * m;

        int max = 0;
        // 尝试转换
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] == 1) continue;
                // 保存四个方向的不相交集根索引
                Set<Integer> set = new HashSet<>();
                for (int k = 0; k < 4; k++) {
                    int newX = i + delta[k], newY = j + delta[k + 1];
                    if (newX >= 0 && newX < n && newY >= 0 && newY < m && grid[newX][newY] == 1) set.add(disJoint.find(newX * m + newY));
                }
                int cur = 1;
                for (int r : set) cur += disJoint.size(r);
                max = Math.max(max, cur);
            }
        }
        return max;
    }
}

class DisJoint {
    int size;
    int[] parent;

    DisJoint(int size) {
        this.size = size;
        this.parent = new int[size];
        Arrays.fill(parent, -1);
    }

    public void union(int r1, int r2) {
        if (parent[r1] >= 0 || parent[r2] >= 0) throw new IllegalArgumentException("invalid param");
        if (r1 == r2) return;
        if (parent[r1] <= parent[r2]) {
            parent[r1] += parent[r2];
            parent[r2] = r1;
        } else {
            parent[r2] += parent[r1];
            parent[r1] = r2;
        }
        size--;
    }

    public int find(int idx) {
        if (parent[idx] < 0) return idx;
        return parent[idx] = find(parent[idx]);
    }

    public int size(int idx) {
        return -parent[find(idx)];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(NM)$。

执行用时：10ms，在所有java提交中击败了73.76%的用户。

内存消耗：38.6MB，在所有java提交中击败了95.81%的用户。

## 官方解题

&emsp;官方思路采用深度优先搜索。