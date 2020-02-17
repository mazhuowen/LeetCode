[toc]

Given a $n \times n$ matrix where each of the rows and columns are sorted in ascending order, find the kth smallest element in the matrix.

Note that it is the kth smallest element in the sorted order, not the kth distinct element.

**Note:**
You may assume k is always valid, $1 \le k \le n^2$.



## 题目解读

&emsp;给定矩阵行列排列都是增序，找到第$k$小元素。

```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        
    }
}
```

## 程序设计

* 观察矩阵，每一个结点后最小的最大元素为其后或其左两个元素之一，可以考虑入队`[0,0]`，每次出队则入队其后、其左元素，出队$k$次，找到第$k$小的值。但这种方法存在一个问题，就是坐标值会重复入队，需要维护一个去重机制。
* 可参考[#373 Find K Pairs with Smallest Sums](./#373 Find K Pairs with Smallest Sums.md)社区方法，将第一行入队，每次出队则入队行索引加一的元素，即其后元素。这样不必去重。

```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        int n = matrix.length;
        // 最小堆
        PriorityQueue<int[]> queue = new PriorityQueue<>(n, (a, b) -> matrix[a[0]][a[1]] - matrix[b[0]][b[1]]);
        // 入队第一行
        for(int i = 0; i < n; i++) {
            queue.add(new int[]{0, i});
        }
        while(!queue.isEmpty() && k > 1) {
            int[] cur = queue.poll();
            // 行索引加一
            if(cur[0] + 1 < n) {
                queue.add(new int[]{cur[0] + 1, cur[1]});
            }
            k--;
        }
        int[] res = queue.poll();
        return matrix[res[0]][res[1]];
    }
}
```

## 性能分析

&emsp;时间复杂度初始化入队$O(N\log_2N)$，后续$k$次出队为$O(k\log_2N)$，总的时间复杂度需要根据$N$和$k$的大小判断，其中$N$为长；空间复杂度为$O(N)$。

执行用时：28ms，在所有java提交中击败了20.60%的用户。

内存消耗：52.9MB，在所有java提交中击败了5.05%的用户。

## 官方解题

&emsp;暂无，密切关注。社区性能较好的方法使用了二分查找思想：

```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        int n = matrix.length;
        // 矩阵的最小值、最大值
        int min = matrix[0][0];
        int max = matrix[n - 1][n - 1];
        // 二分查找
        while(min < max) {
            // 每次循环都保证第K小的数在min~max之间，直到min与max相等，就是第k小的数
            int mid = (min + max) / 2;
            // 找二维矩阵中小于等于mid的元素总数
            int count = count(matrix, n, mid);
            if(count < k) {
                // 第k小的数在右半部分，且不包含mid
                min = mid + 1;
            } else {
                // 第k小的数在左半部分，可能包含mid
                max = mid;
            }
        }
        return min;
    }

    private int count(int[][] matrix, int n, int mid) {
        int row = 0;
        int col = n - 1;
        int count = 0;
        // 从第一行开始，逐行检查
        while(row < n && col >= 0) {
            // 当前行col位置元素小于等于mid，计数并迭代下一行
            if(matrix[row][col] <= mid) {
                count += col + 1;
                row++;
            } 
            // 当前行col位置元素大于mid，列减少，继续在当前行迭代查找
            else {
                col--;
            }
        }
        // 返回总数
        return count;
    }
}
```

&emsp;时间复杂度二分需要花费$O(\log_2(\text{max} - \text{min}))$，每次二分查找不大于$\text{mid}$的个数需要花费$O(N)$的时间，总的时间复杂度为$O(N\log_2(\text{max} - \text{min}))$；空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：54.6MB，在所有java提交中击败了5.05%的用户。