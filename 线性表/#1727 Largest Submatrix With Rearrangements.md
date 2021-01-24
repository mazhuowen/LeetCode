[toc]

You are given a binary `matrix` matrix of size $m \times n$, and you are allowed to rearrange the **columns** of the `matrix` in any order.

Return the area of the largest submatrix within `matrix` where **every** element of the submatrix is $1$ after reordering the columns optimally.

 

**Example 1**:

<img src="../images/#1727_exp1.png" style="zoom: 67%;" />

```
Input: matrix = [[0,0,1],[1,1,1],[1,0,1]]
Output: 4
Explanation: You can rearrange the columns as shown above.
The largest submatrix of 1s, in bold, has an area of 4.
```

**Example 2**:

<img src="../images/#1727_exp2.png" style="zoom: 67%;" />

```
Input: matrix = [[1,0,1,0,1]]
Output: 3
Explanation: You can rearrange the columns as shown above.
The largest submatrix of 1s, in bold, has an area of 3.
```

**Example 3**:

```
Input: matrix = [[1,1,0],[1,0,1]]
Output: 2
Explanation: Notice that you must rearrange entire columns, and there is no way to make a submatrix of 1s larger than an area of 2.
```

**Example 4**:

```
Input: matrix = [[0,0],[0,0]]
Output: 0
Explanation: As there are no 1s, no submatrix of 1s can be formed and the area is 0.
```



**Constraints**:

* $m == \text{matrix.length}$
* $n == \text{matrix[i].length}$
* $1 \le m * n \le 10^5$
* `matrix[i][j]` is $0$ or $1$.



## 题目解读

&emsp;求交换列后的最大全$1$矩形面积。

```java
class Solution {
    public int largestSubmatrix(int[][] matrix) {

    }
}
```

## 程序设计

* 以当前行为起始，采用集合记录当前行及之后行是$1$的列，并计算面积。

```java
class Solution {
    public int largestSubmatrix(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        int max = 0;
        // 记录当前是1的列，和待删除的列
        Set<Integer> cols = new HashSet<>(), remove = new HashSet<>();
        
        point: for (int i = 0; i < m; i++) {
            // 初始当前行的是1的列
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 1) cols.add(j);
            }
            max = Math.max(max, cols.size());
            
            for (int j = i + 1; j < m; j++) {
                // 不是1,加入待删除集合
                for (int k : cols) {
                    if (matrix[j][k] == 0) remove.add(k);
                }
                // 删除并计算
                cols.removeAll(remove);
                remove.clear();
                if (cols.isEmpty()) continue point;
                max = Math.max(max, (j - i + 1) * cols.size());
            }
            // 剪枝
            if (max >= (m - i - 1) * n) break;
            cols.clear();
        }
        return max;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(M^2N)$，空间复杂度为$O(N)$。

执行用时：42 ms, 在所有 Java 提交中击败了10.79%的用户。

内存消耗：62 MB, 在所有 Java 提交中击败了30.84%的用户。

## 官方解题

&emsp;暂无，密切关注。社区采用统计连续为$1$的列的长度，然后排序计算面积的思路。

```java
class Solution {
    public int largestSubmatrix(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;

        // 统计连续为1的列的长度
        for (int i = 1; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 1) matrix[i][j] += matrix[i - 1][j];
            }
        }

        // 计算最大面积
        int max = 0;
        for (int i = 0; i < m; i++) {
            // 排序
            Arrays.sort(matrix[i]);
            for (int j = n - 1; j >= 0 && matrix[i][j] > 0; j--) {
                max = Math.max(max, (n - j) * matrix[i][j]);
            }
        }
        
        return max;
    }
}
```

&emsp;时间复杂度为$O(MN\log_2N)$，空间复杂度为$O(1)$。

执行用时：8 ms, 在所有 Java 提交中击败了97.57%的用户。

内存消耗：58.5 MB, 在所有 Java 提交中击败了57.59%的用户。