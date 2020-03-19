[toc]

You are given a map of a server center, represented as a `m * n` integer matrix `grid`, where $1$ means that on that cell there is a server and 0 means that it is no server. Two servers are said to communicate if they are on the same row or on the same column.

Return the number of servers that communicate with any other server.



**Constraints:**

- $m == \text{grid.length}$
- $n == \text{grid[i].length}$
- $1 \le m \le 250$
- $1 \le n \le 250$
- $\text{grid[i][j]} == 0$ or $1$



## 题目解读

&emsp;给定服务器分布矩形，同一横坐标、纵坐标的服务器可以通信，给出可以通信的服务器。

```java
class Solution {
    public int countServers(int[][] grid) {

    }
}
```

## 程序设计

* 当服务器是单台时，无法通信，则可通信服务器是总的服务器减去单台服务器。可以使用不相交集来实现，每个点都将一行和一列关联起来，对于单台服务器所在的不相交集，只有横坐标和纵坐标，对于服务器集群，则有多于两个坐标存在，这样合并后可以判断哪些是单台服务器，哪些是集群。

```java
class Solution {
    public int countServers(int[][] grid) {
        if (grid == null || grid.length == 0) return 0;
        int m = grid.length;
        int n = grid[0].length;
        // 行和列的不相交集
        DisJoint disJoint = new DisJoint(m + n);
        // 记录总的服务器
        int count = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // 关联行列（列的索引从m开始）
                if (grid[i][j] == 1) {
                    disJoint.union(disJoint.find(i), disJoint.find(m + j));
                    count++;
                }
            }
        }
        // 总的服务器数减去单台服务区数目
        return count - disJoint.count(2);
    }
}

class DisJoint {
    int[] tree;

    DisJoint(int size) {
        this.tree = new int[size];
        Arrays.fill(tree, -1);
    }

    public void union(int root1, int root2) {
        if (tree[root1] >= 0 || tree[root2] >= 0) throw new IllegalArgumentException("not a root");
        if (root1 == root2) return;

        if (tree[root1] <= tree[root2]) {
            tree[root1] += tree[root2];
            tree[root2] = root1;
        } else {
            tree[root2] += tree[root1];
            tree[root1] = root2;
        }
    }

    public int find(int idx) {
        if (tree[idx] < 0) return idx;
        return tree[idx] = find(tree[idx]);
    }
    // 计算尺寸为size的集合个数
    public int count(int size) {
        int count = 0;
        for (int i = 0; i < tree.length; i++) {
            if (tree[i] == -size) count++;
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(M + N)$。

执行用时：3ms，在所有java提交中击败了94.94%的用户。

内存消耗：46.7MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方提出了另一种思路，单纯的从数组出发，通过行、列数组记录每一行、每一列的服务器数目；这样再次遍历矩形，如果当前服务器的行、列上总的服务器数为1，则表示只有一台服务器，找不到可通信的服务器。这样就可以得到能通信的服务器数目。

```java
class Solution {
    public int countServers(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[] row = new int[m]; // 统计某一行的服务器数量
        int[] col = new int[n]; // 统计某一列的服务器数量
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 0) continue;
                row[i] += 1; // 第 i 行服务器数量+1
                col[j] += 1; // 第 j 列服务器数量+1
            }
        }
        int ans = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 0) continue; 
                // 该行该列的服务器数量只有一个（即其自身）则它无法与任何其他服务器通信
                if (row[i] == 1 && col[j] == 1) continue; 
                ans++;
            }
        }
        return ans;
    }
}
```

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(M + N)$。

执行用时：2ms，在所有java提交中击败了99.72%的用户。

内存消耗：46.6MB，在所有java提交中击败了100.00%的用户。