[toc]

Given a 2D board containing `X` and `O` (**the letter O**), capture all regions surrounded by `X`.

A region is captured by flipping all `O`s into `X`s in that surrounded region.



Explanation:

Surrounded regions shouldn’t be on the border, which means that any `O` on the border of the board are not flipped to `X`. Any `O` that is not on the border and it is not connected to an `O` on the border will be flipped to `X`. Two cells are connected if they are adjacent cells connected horizontally or vertically.



## 题目解读

&emsp;给定矩形，将被`X`包围的`O`置为`X`。

```java
class Solution {
    public void solve(char[][] board) {

    }
}
```

## 程序设计

* 首先定义不相交集：

```java
class DisJoint {
    private int[] tree;

    DisJoint(int size) {
        this.tree = new int[size];
        // 初始化为size个不相交集
        for(int i = 0; i < size; i++) {
            tree[i] = -1;
        }
    }
    // 按规模并
    public void union(int root1, int root2) {
        if(tree[root1] >= 0 || tree[root2] >= 0) {
            throw new IllegalArgumentException("not a root");
        }
        if(root1 == root2) {
            return;
        }
        // 树2大，树1并入树2
        if(tree[root1] >= tree[root2]) {
            tree[root2] += tree[root1];
            tree[root1] = root2;
        } else {
            tree[root1] += tree[root2];
            tree[root2] = root1;
        }
    }
    // 路径压缩
    public int find(int val) {
        if(tree[val] < 0) {
            return val;
        }
        return tree[val] = find(tree[val]);
    }
}
```

* 使用不相交集记录边界结点。

```java
class Solution {
    public void solve(char[][] board) {
        if(board == null || board.length == 0) {
            return;
        }
        int m = board.length, n = board[0].length;
        DisJoint disJoint = new DisJoint(m * n + 1);
        // 设置0为通向边界的O的集合
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                char c = board[i][j];
                int idx = i * n + j + 1;
                if(c == 'O') {
                    // 边界点加入标识为0的集合
                    if(i == 0 || i == m - 1 || j == 0 || j == n - 1) {
                            disJoint.union(disJoint.find(0), disJoint.find(idx));
                    }
                    // 前面的O合并
                    if(j - 1 >= 0 && board[i][j - 1] == 'O') {
                        disJoint.union(disJoint.find(idx - 1), disJoint.find(idx));
                    }
                    // 上面的O合并
                    if(i - 1 >= 0 && board[i - 1][j] == 'O') {
                        disJoint.union(disJoint.find(idx - n), disJoint.find(idx));
                    }
                }
            }
        }
		// 遍历O将非边界点置为X
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(board[i][j] == 'O' && disJoint.find(0) != disJoint.find(i * n + j + 1)) {
                    board[i][j] = 'X';
                }
            }
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(NM)$。

执行用时：11ms，在所有java提交中击败了12.26%的用户。

内存消耗：41.8MB，在所有java提交中击败了33.38%的用户。

## 官方解题

&emsp;暂无，密切关注。社区时间性能较快的方法采用边界点广度优先搜索的方法。

```java
class Solution {
    public void solve(char[][] board) {
        int row = board.length;
        if(row == 0){
            return ;
        }

        int col = board[0].length;
        // 记录是否访问过
        boolean[][] visited = new boolean[row][col];
        // 遍历边界，并广度优先搜索
        // 遍历第一列和最后一列
        for(int i = 0; i < col; i++){
            if(board[0][i] == 'O' && !visited[0][i]) dfs(0, i, board, visited);
            if(board[row-1][i] == 'O' && !visited[row-1][i]) dfs(row-1, i, board, visited);
        }
        // 遍历第一行和最后一行
        for(int i = 0; i < row; i++){
            if(board[i][0]== 'O' && !visited[i][0]) dfs(i, 0, board, visited);
            if(board[i][col-1]== 'O' && !visited[i][col-1]) dfs(i, col-1, board, visited);
        }
        // 剩下的是边界点不可达的点，置为O
        for(int i = 0; i < row; i++){
            for(int j = 0; j < col; j++){
                if(!visited[i][j]){
                    board[i][j] = 'X';
                }
            }
        }
    }

    public void dfs(int i, int j, char[][] board, boolean[][] visited){
        if(i < 0 || i >= board.length || j < 0 || j >= board[0].length || visited[i][j]){
            return;
        }
        // 扩展搜索
        if(board[i][j] == 'O'){
            visited[i][j] = true;
            dfs(i+1, j, board, visited);
            dfs(i-1, j, board, visited);
            dfs(i, j+1, board, visited);
            dfs(i, j-1, board, visited);
        }
    }
}
```

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(NM)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：42.1MB，在所有java提交中击败了33.10%的用户。