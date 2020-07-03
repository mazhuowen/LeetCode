[toc]

An $N \times N$ `board` contains only `0`s and `1`s. In each move, you can swap any 2 rows with each other, or any 2 columns with each other.

What is the minimum number of moves to transform the board into a "chessboard" - a board where no `0`s and no `1`s are 4-directionally adjacent? If the task is impossible, return -1.



Note:

* `board` will have the same number of rows and columns, a number in the range `[2, 30]`.
* `board[i][j]` will be only `0`s or `1`s.



## 题目解读

&emsp;求变为棋盘的最小数目，棋盘定义为任意一格的上下左右四个方向均与本身不同的矩阵。

```java
class Solution {
    public int movesToChessboard(int[][] board) {

    }
}
```

## 程序设计

* 参考官方解题，首先注意到交换行，类的种数不变，交换列，行的种数不变，而且种数只能是$2$；其此两种类型数目相差之能是一；最后每中组合行或列中$0$、$1$数目相差不超过$1$。
* 交换次数可通过亦或操作获取。

```java
class Solution {
    public int movesToChessboard(int[][] board) {
        if (board == null || board.length != board[0].length) throw new IllegalArgumentException("invalid param");

        int n = board.length;
        Integer v1 = null, v2 = null;
        // 统计行的二进制表示与计数
        Map<Integer, Integer> counter = new HashMap<>();
        for (int[] row : board) {
            int r = 0;
            for (int num : row) r = (r << 1) | num;
            counter.put(r, counter.getOrDefault(r, 0) + 1); 
            if (v1 == null) v1 = r;
            else if (r != v1) v2 = r;
        }
        // 交换列
        int count1 = swapArray(counter, v1, v2, n);
        if (count1 == -1) return -1;

        // 统计列的二进制表示并计数
        v1 = null;
        counter.clear();
        for (int i = 0; i < n; i++) {
            int c = 0;
            for (int j = 0; j < n; j++) {
                c = (c << 1) | board[j][i];
            }
            counter.put(c, counter.getOrDefault(c, 0) + 1); 
            if (v1 == null) v1 = c;
            else if (c != v1) v2 = c;
        }
        // 交换行
        int count2 = swapArray(counter, v1, v2, n);
        if (count2 == -1) return -1;
        return count1 + count2;
    }

    // 返回交换的数目
    private int swapArray(Map<Integer, Integer> counter, Integer v1, Integer v2, int n) {
        int mask = (1 << n) - 1;
        // 行或列只有两种
        if (counter.size() != 2) return -1;
        // 行或列必须互补
        if ((v1 ^ v2) != mask) return -1;

        int ones = Integer.bitCount(v1), zeros = n - ones;
        // 行列中1或0的数目
        if (Math.abs(ones - zeros) > 1) return -1; 
        // 行或列的数目是否满足条件
        if (counter.get(v1) == n / 2 && counter.get(v2) == (n + 1) / 2 
            || counter.get(v1) == (n + 1) / 2 && counter.get(v2) == n / 2) {
                int count = Integer.MAX_VALUE;
                // 交换列或行(传入的是行或列)
                // 1结尾即……010101
                if (ones >= zeros) count = Math.min(count, Integer.bitCount(v1 ^ 0x55555555 & mask) / 2);
                // 0结尾即……101010
                if (zeros >= ones) count = Math.min(count, Integer.bitCount(v1 ^ 0xAAAAAAAA & mask) / 2);
                
                return count;
            }
        return -1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了60.42%的用户。

内存消耗：39.6MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;见上。