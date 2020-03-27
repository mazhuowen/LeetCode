[toc]

Given a matrix of integers `A` with R rows and C columns, find the **maximum** score of a path starting at `[0,0]` and ending at `[R-1,C-1]`.

The score of a path is the **minimum** value in that path.  For example, the value of the path $8 \to  4 \to  5 \to  9$ is $4$.

A path moves some number of times from one visited cell to any neighbouring unvisited cell in one of the 4 cardinal directions (north, east, west, south).



**Note:**

* $1 \le R, C \le 100$
* $0 \le \text{A[i][j]} \le 10^9$



## 题目解读

&emsp;路径的得分是该路径上的最小值。找出所有路径中得分最高的那条路径，返回其得分。

```java
class Solution {
    public int maximumMinimumPath(int[][] A) {

    }
}
```

## 程序设计

* 最基本的解法是深度优先搜索，穷举遍历，但是会超时。矩阵中每个位置可看做一个点，每个点有两个边或四个边连接到另外的点，由于每个点不能回溯，这样平均每个点都三条边可选，所有路径有$3^{MN}$种可能性，时间复杂度就是$O(3^{MN})$。

```java
class Solution {
    public int maximumMinimumPath(int[][] A) {
        if (A == null || A.length == 0) throw new IllegalArgumentException("invalid Param");
        boolean[][] visited = new boolean[A.length][A[0].length];
        return dfs(0, 0, A, visited);

    }

    private int dfs(int x, int y, int[][] A, boolean[][] visited) {
        // 已经遍历过，返回-1
        if (visited[x][y]) return -1;
        // 终点
        if (x == A.length - 1 && y == A[0].length - 1) return A[x][y];

        // 标记为正在访问
        visited[x][y] = true;

        int[] dx = new int[]{-1, 1, 0, 0};
        int[] dy = new int[]{0, 0, -1, 1};
        // 子路径中最大的最小路径
        int maxPath = -1;
        for (int i = 0; i < 4; i++) {
            int newX = x + dx[i], newY = y + dy[i];
            if (newX < 0 || newX >= A.length || newY < 0 || newY >= A[0].length
                    || visited[newX][newY]) continue;

            int childVal = dfs(newX, newY, A, visited);
            if (childVal != -1) {
                maxPath = Math.max(maxPath, childVal);
            }
        }

        visited[x][y] = false;
        return temp == -1 ? -1 : Math.min(maxPath, A[x][y]);
    }
}
```

* 仔细观察题目示例，该问题实际与迷宫寻找出路很类似，将矩形中值最大的位置加入路径，直到首尾连通，因为贪心策略，此时找到的路径必然是最大的最小路径。
* 考虑到按照位置的值的大小选择，有些不与最后的路径重叠，是孤点或隔离的区域；有些则形成分叉，不会有路径延伸出去。这类点是否会影响路径的判断？由于选择是贪心策略，每次选剩余位置最大的点，如果这两类点不满足条件，则满足条件的点必然是这类点之后选的，必然小于等于这类点，即不影响最小路径的值的判断。
* 为了保存路径的最小值，需要对不相交集做改动，每个根除了保存不相交集的尺寸，还要保存集合内最小的值。最后返回这个值就是答案。

```java
class Solution {
    public int maximumMinimumPath(int[][] A) {
        if (A == null || A.length == 0) throw new IllegalArgumentException("invalid param");

        int row = A.length, col = A[0].length;
        // 保存索引和值，最大堆
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> b[2] - a[2]);
        DisJoint disJoint = new DisJoint(row * col);
        
        // 遍历矩形入队
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                int idx = i * col + j;
                queue.add(new int[]{i, j, A[i][j]});
                disJoint.upate(idx, A[i][j]);
            }
        }
        int start = 0, end = row * col - 1;
        boolean[][] visited = new boolean[row][col];
        int[] dx = new int[]{-1, 1, 0, 0};
        int[] dy = new int[]{0, 0, -1, 1};
        // 选择最大边出队并加入，直到首尾连接，结束循环
        while (disJoint.find(start) != disJoint.find(end)) {
            int[] cur = queue.poll();
            // 标记为加入路径
            visited[cur[0]][cur[1]] = true;

            for (int i = 0; i < 4; i++) {
                int x = cur[0] + dx[i];
                int y = cur[1] + dy[i];
                if (x < 0 || x >= row || y < 0 || y >= col || !visited[x][y]) continue;
                // 和已经遍历过的点合并扩展
                disJoint.union(disJoint.find(cur[0] * col + cur[1]), disJoint.find(x * col + y));
            }
        }
        return disJoint.getMinVal(start);
    }
}

class DisJoint {
    int[][] tree;

    DisJoint(int size) {
        this.tree = new int[size][2];
    }

    // 更新每个结点值
    public void upate(int idx, int val) {
        tree[idx][0] = -1;
        tree[idx][1] = val;
    }

    public void union(int root1, int root2) {
        if (tree[root1][0] >= 0 || tree[root2][0] >= 0) throw new IllegalArgumentException("invalid param");
        if (root1 == root2) return;

        if (tree[root1][0] <= tree[root2][0]) {
            tree[root1][0] += tree[root2][0];
            tree[root2][0] = root1;
            tree[root1][1] = Math.min(tree[root1][1], tree[root2][1]);
        } else {
            tree[root2][0] += tree[root1][0];
            tree[root1][0] = root2;
            tree[root2][1] = Math.min(tree[root1][1], tree[root2][1]);
        }
    }

    public int find(int idx) {
        if (tree[idx][0] < 0) return idx;
        return tree[idx][0] = find(tree[idx][0]);
    }

    public int getMinVal(int idx) {
        return tree[find(idx)][1];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN\log_2MN)$，空间复杂度为$O(MN)$。

执行用时：161ms，在所有java提交中击败了47.71%的用户。

内存消耗：42MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区有非常巧妙的做法，使用二分来确定路径最小值，然后通过方法确认是否存在这样的路径，来缩小遍历范围。但是该算法不稳定，非常依赖数据。

```java
class Solution {
    private int[] delta = new int[]{0, 1, 0, -1, 0};

    public int maximumMinimumPath(int[][] A) {
        if (A == null || A.length == 0) throw new IllegalArgumentException("invalid param");

        int row = A.length, col = A[0].length;
        
        // 路径值最小是0，最大是起点和终点的最小值
        int left = 0, right = Math.min(A[0][0], A[row - 1][col - 1]) + 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            // 深度遍历标识
            boolean[][] visited = new boolean[row][col];
            // 存在通往起点和终点的路径上值都大于mid值，区间向上搜索
            if (exit(0, 0, row - 1, col - 1, mid, A, visited)) {
                left = mid + 1;
            } 
            // 路径存在小于mid值，向下搜索
            else {
                 right = mid;
            }
        }
        return left - 1;
    }

    private boolean exit(int startX, int startY, int endX, int endY, int limit, int[][] A, boolean[][] visited) {
        // 到达终点，连通
        if (startX == endX && startY == endY) return true;

        // 标记为正在访问
        visited[startX][startY] = true;
        for (int i = 0; i < 4; i++) {
            int x = startX + delta[i], y = startY + delta[i + 1];
            // 坐标不合法或已访问或存在比要求值小的值
            if (x < 0 || x > endX || y < 0 || y > endY || visited[x][y] || A[x][y] < limit) continue;
            // 递归，有满足条件的直接返回
            if (exit(x, y, endX, endY, limit, A, visited)) {
                return true;
            }
        }
        return false;
    }
}
```

&emsp;二分查找是常数，主要讨论遍历；时间复杂度最好的情况，也就是起点周围都是不满足条件的点的情况下是$O(1)$，如果只存在一条满足条件的情况时间复杂度不超过$O(MN)$，最坏情况是终点四周的点不满足条件，这样要遍历$O(3^{MN})$，这是因为这样该算法对任何消耗时间的操作敏感，比如在上述功能中每次不新建`visited`数组，而是在递归遍历结束时重新置为`false`，算法直接超时，可见递归的层数之深，简单的数组读写都影响很大；其次对于坐标使用两个数组`dx`、`dy`表示偏移会比上面使用一个慢60ms；空间复杂度为$O(MN)$。总之就运行平均时间来说是最优算法，但从稳定性等角度考虑，不相交集的算法最好。

执行用时：23ms，在所有java提交中击败了93.58%的用户。

内存消耗：43.7MB，在所有java提交中击败了100.00%的用户。