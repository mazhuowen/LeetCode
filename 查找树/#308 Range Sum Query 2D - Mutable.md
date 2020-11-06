[toc]

Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner `(row1, col1)` and lower right corner `(row2, col2)`.



**Note**:

* The matrix is only modifiable by the update function.
* You may assume the number of calls to `update` and `sumRegion` function is distributed evenly.
* You may assume that $row1 \le row2$ and $col1 \le col2$.



## 题目解读

&emsp;同[#307 Range Sum Query - Mutable](./#307 Range Sum Query - Mutable.md)，只是本题中是矩形。

```java
class NumMatrix {

    public NumMatrix(int[][] matrix) {

    }
    
    public void update(int row, int col, int val) {

    }
    
    public int sumRegion(int row1, int col1, int row2, int col2) {

    }
}

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix obj = new NumMatrix(matrix);
 * obj.update(row,col,val);
 * int param_2 = obj.sumRegion(row1,col1,row2,col2);
 */
```

## 程序设计

* 同[#307 Range Sum Query - Mutable](./#307 Range Sum Query - Mutable.md)创建线段树，只是检索变成了多个区间，而不是一个区间。

```java
class NumMatrix {
    // 矩形尺寸
    private int m;
    private int n;
    // 线段树
    private int[] tree;

    public NumMatrix(int[][] matrix) {
        if((m = matrix.length) > 0 && (n = matrix[0].length) > 0) {
            tree = new int[2 * m * n];
            // 初始化叶结点
            for(int i = 0; i < m; i++) {
                for(int j = 0; j < n; j++) {
                    tree[m * n + i * n + j] = matrix[i][j];
                }
            }
            // 构建线段树
            for(int i = m * n - 1; i > 0; i--) {
                tree[i] = tree[2 * i] + tree[2 * i + 1];
            }
        }
    }
    
    public void update(int row, int col, int val) {
        // 不合法
        if(row < 0 || row >= m || col < 0 || col >= n) {
            return;
        }
        // 记录差值
        int index = m * n + row * n + col;
        // 更新叶节点
        tree[index] = val;
        // 更新父节点
        int p = index / 2;
        while(p > 0) {
            int left = index, right = index;
            // 子节点为左结点
            if(index % 2 == 0) {
                right = index + 1;
            } else {
                left = index - 1;
            }
            tree[p] = tree[left] + tree[right];
            // 迭代
            index = p;
            p /= 2;
        }
    }
    
    public int sumRegion(int row1, int col1, int row2, int col2) {
        // 不合法
        if(row1 < 0 || row2 >= m || col1 < 0 || col2 >= n) {
            return 0;
        }
        int sum = 0;
        for(int row = row1; row <= row2; row++) {
            sum += sumRegion(row, col1, col2);
        }
        return sum;
    }
    // 查询一行区间的和
    private int sumRegion(int row, int col1, int col2) {
        int left = m * n + row * n + col1;
        int right = m * n + row * n + col2;
        int sum = 0;
        while(left <= right) {
            // 下界为右结点
            if(left % 2 == 1) {
                sum += tree[left];
                left++;
            }
            // 上界为左结点
            if(right % 2 == 0) {
                sum += tree[right];
                right--;
            }
            left /= 2;
            right /= 2;
        }
        return sum;
    }
}
```

## 性能分析

&emsp;创建线段树的时间复杂度为$O(M*N)$，空间复杂度为$O(M*N)$。更新值的时间复杂度为$O(\log_2M*N)$，空间复杂度为$O(1)$。统计的时间复杂度为$O(M\log_2M*N)$，空间复杂度为$O(1)$。

执行用时：29ms，在所有java提交中击败了51.95%的用户。

内存消耗：47.7MB，在所有java提交中击败了6.25%的用户。

## 官方解题

&emsp;暂无，密切关注。社区时间性能较好的方法采用前缀和的思路。

```java
class NumMatrix {
    // 矩形尺寸
    private int m;
    private int n;
    private int[][] matrix;
    // 前缀和
    private int[][] preSum;

    public NumMatrix(int[][] matrix) {
        if((m = matrix.length) > 0 && (n = matrix[0].length) > 0) {
            this.matrix = matrix;
            this.preSum = new int[m][n + 1];
            // 计算前缀和
            for(int i = 0; i < m; i++) {
                for(int j = 0; j < n; j++) {
                    preSum[i][j + 1] = matrix[i][j] + preSum[i][j];
                }
            }
        }
    }
    
    public void update(int row, int col, int val) {
        // 不合法
        if(row < 0 || row >= m || col < 0 || col >= n) {
            return;
        }
        // 更新矩阵
        matrix[row][col] = val;
        // 更新前缀和
        for(int j = col; j < n; j++) {
            preSum[row][j + 1] = preSum[row][j] + matrix[row][j];
        }
    }
    
    public int sumRegion(int row1, int col1, int row2, int col2) {
        // 不合法
        if(row1 < 0 || row2 >= m || col1 < 0 || col2 >= n) {
            return 0;
        }
        int sum = 0;
        for(int row = row1; row <= row2; row++) {
            sum += preSum[row][col2 + 1] - preSum[row][col1];
        }
        return sum;
    }
}
```

&emsp;构建前缀和时间复杂度为$O(M*N)$，空间复杂度为$O(M*N)$。更新时间复杂度为$O(N)$，空间复杂度为$O(1)$。统计时间复杂度为$O(M)$，空间复杂度为$O(1)$。

执行用时：16ms，在所有java提交中击败了100.00%的用户。

内存消耗：47.4MB，在所有java提交中击败了6.25%的用户。