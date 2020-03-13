[toc]

A 2d grid map of m rows and n columns is initially filled with water. We may perform an addLand operation which turns the water at position (row, col) into a land. Given a list of positions to operate, **count the number of islands after each addLand operation**. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

Follow up:

Can you do it in time complexity $O(k\log_2mn)$, where $k$ is the length of the positions?



## 题目解读

&emsp;依次在方格中添加陆地，并输出每次添加后的岛屿个数。

```java
class Solution {
    public List<Integer> numIslands2(int m, int n, int[][] positions) {

    }
}
```

## 程序设计

* 由于是动态添加，使用深度优先遍历等思路显得不那么优雅。可以使用不相交集，每次加入陆地，则查找其上下左右的格子是否是陆地，是则合并，并返回此时的岛屿数。为了记录是否是陆地，可以使用矩形记录。

```java
class Solution {
    public List<Integer> numIslands2(int m, int n, int[][] positions) {
        List<Integer> res = new LinkedList<>();
        if(m == 0 || n == 0 || positions == null || positions.length == 0) {
            return res;
        }
        // 水的数目
        int count = m * n;
        // 记录陆地
        boolean[][] island = new boolean[m][n];
        DisJoint disJoint = new DisJoint(count);
        for(int[] cur : positions) {
            int x = cur[0], y = cur[1];
            int idx = x * n + y;
            // 避免重复点操作导致计数重复减少
            if(!island[x][y]) {
                count--;
            }
            island[x][y] = true;
            // 检查前后，如果是陆地，则合并
            if(y - 1 >= 0 && island[x][y - 1]) {
                disJoint.union(disJoint.find(idx - 1), disJoint.find(idx));
            }
            if(y + 1 < n && island[x][y + 1]) {
                disJoint.union(disJoint.find(idx + 1), disJoint.find(idx));
            }
            // 检查上下，如果是陆地，则合并
            if(x - 1 >= 0 && island[x - 1][y]) {
                disJoint.union(disJoint.find(idx - n), disJoint.find(idx));
            }
            if(x + 1 < m && island[x + 1][y]) {
                disJoint.union(disJoint.find(idx + n), disJoint.find(idx));
            }
            // 添加当前岛屿数
            res.add(disJoint.size() - count);
        }
        return res;
    }
}

class DisJoint {
    private int[] tree;
    private int size;

    DisJoint(int size) {
        this.size = size;
        this.tree = new int[size];
        // 填充-1
        Arrays.fill(tree, -1);
    }

    public void union(int root1, int root2) {
        if(tree[root1] >= 0 || tree[root2] >= 0) throw new IllegalArgumentException("root must be negative");
        if(root1 == root2) return;
        if(tree[root1] < tree[root2]) {
            tree[root1] += tree[root2];
            tree[root2] = root1;
        } else {
            tree[root2] += tree[root1];
            tree[root1] = root2;
        }
        size--;
    }

    public int find(int idx) {
        if(tree[idx] < 0) return idx;
        return tree[idx] = find(tree[idx]);
    }

    public int size() {
        return size;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(M*N + K)$，空间复杂度为$O(N* M)$。

执行用时：10ms，在所有java提交中击败了98.80%的用户。

内存消耗：44.3MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;除了上述思路，官方还提供了哈希表的实现。