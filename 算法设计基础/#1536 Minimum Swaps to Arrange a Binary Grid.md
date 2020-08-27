[toc]

Given an $n \times n$ binary `grid`, in one step you can choose two **adjacent rows** of the grid and swap them.

A grid is said to be **valid** if all the cells above the main diagonal are **zeros**.

Return the minimum number of steps needed to make the grid valid, or $-1$ if the grid cannot be valid.

The main diagonal of a grid is the diagonal that starts at cell $(1, 1)$ and ends at cell $(n, n)$.



**Constraints**:

* $n == \text{grid.length}$
* $n == \text{grid[i].length}$
* $1 \le n \le 200$
* `grid[i][j]` is $0$ or $1$



## 题目解读

&emsp;给定正方形，要求交换后对角线上的格子值为$1$，如果无法交换得到则返回$-1$。

```java
class Solution {
    public int minSwaps(int[][] grid) {
        
    }
}
```

## 程序设计

* 首先统计每行右边连续的$1$的数目，将二维问题转化为一维问题；然后采用贪婪策略进行交换排序。

```java
class Solution {
    public int minSwaps(int[][] grid) {
        if (grid == null || grid.length != grid[0].length) throw new IllegalArgumentException("invalid param");

        int n = grid.length;
        // 统计数目
        int[] zero = new int[n];
        for (int i = 0; i < n; i++) {
            zero[i] = count(grid[i]);
        }

        int step = 0;
        for (int i = 0; i < n - 1; i++) {
            // 满足条件（由于0的个数从第一行到最后一行递减，贪婪的，如果当前数目超过所需数目，则不必交换）
            if (zero[i] >= n - i - 1) continue;
            // 查找符合条件的第一个行
            int j = i;
            for (; j < n; j++) {
                if (zero[j] >= n - i - 1) break; 
            }

            // 无法满足当前行
            if (j >= n) return -1;

            // 交换i与j
            for (; j > i; j--) {
                swap(zero, j, j - 1);
                step++;
            }
        }

        return step;
    }

    private int count(int[] row) {
        int num = 0;
        for (int i = row.length - 1; i >= 0; i--) {
            if (row[i] == 1) break;
            num++;
        }
        return num;
    }

    private void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了56.61%的用户。

内存消耗：42.3MB，在所有java提交中击败了55.15%的用户。

## 官方解题

&emsp;见上。