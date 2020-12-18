[toc]

Given an $m \times n$ `matrix`, return a new matrix `answer` where `answer[row][col]` is the **rank** of `matrix[row][col]`.

The **rank** is an **integer** that represents how large an element is compared to other elements. It is calculated using the following rules:

* The rank is an integer starting from $1$.
* If two elements `p` and `q` are in the **same row or column**, then:
  * If $p < q$ then $\text{rank(p)} < \text{rank(q)}$
  * If $p == q$ then $\text{rank(p)} == \text{rank(q)}$
  * If $p > q$ then $\text{rank(p)} > \text{rank(q)}$
* The **rank** should be as **small** as possible.

It is guaranteed that `answer` is unique under the given rules.

 

**Example 1**:

<img src="..\images\#1632_exp1.jpg" style="zoom:80%;" />

```
Input: matrix = [[1,2],[3,4]]
Output: [[1,2],[2,3]]
Explanation:
The rank of matrix[0][0] is 1 because it is the smallest integer in its row and column.
The rank of matrix[0][1] is 2 because matrix[0][1] > matrix[0][0] and matrix[0][0] is rank 1.
The rank of matrix[1][0] is 2 because matrix[1][0] > matrix[0][0] and matrix[0][0] is rank 1.
The rank of matrix[1][1] is 3 because matrix[1][1] > matrix[0][1], matrix[1][1] > matrix[1][0], and both matrix[0][1] and matrix[1][0] are rank 2.
```

**Example 2**:

<img src="..\images\#1632_exp2.jpg" style="zoom:80%;" />

```
Input: matrix = [[7,7],[7,7]]
Output: [[1,1],[1,1]]
```

**Example 3**:

<img src="..\images\#1632_exp3.jpg" style="zoom:80%;" />

```
Input: matrix = [[20,-21,14],[-19,4,19],[22,-47,24],[-19,4,19]]
Output: [[4,2,3],[1,3,4],[5,1,6],[1,3,4]]
```

**Example 4**:

<img src="..\images\#1632_exp4.jpg" style="zoom:80%;" />

```
Input: matrix = [[7,3,6],[1,4,5],[9,8,2]]
Output: [[5,1,4],[1,2,3],[6,3,1]]
```



**Constraints**:

* $m == \text{matrix.length}$
* $n == \text{matrix[i].length}$
* $1 \le m, n \le 500$
* $-10^9 \le \text{matrix[row][col]} \le 10^9$



## 题目解读

&emsp;求给定矩阵的秩。

```java
class Solution {
    public int[][] matrixRankTransform(int[][] matrix) {

    }
}
```

## 程序设计

* 以示例2为例，可将矩阵数值排序，并记录每行、列当前的最大秩；这样$-47$最小，最先分配，分配秩为$1$，$-21$次小，由于$-21$所在列已经分配了$1$，故$-21$分配$2$；接着是$-19$，由于$-19$有两个，其所在的行列都未分配值，故分配$1$，依次类推得到矩阵的秩；

* 为了实现上述思路，首先使用不相交集来将同行、列的相等的值归类，并维护索引到同一个链表，然后根据矩阵值排序，贪婪的从最小值开始分配。

```java
class Solution {
    public int[][] matrixRankTransform(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        Integer[] index = new Integer[m * n];
        for (int i = 0; i < index.length; i++) index[i] = i;
        // 排序
        Arrays.sort(index, (a, b) -> matrix[a / n][a % n] - matrix[b / n][b % n]); 
        // 将同一行、列相等得值加入不想交集
        DisJoint disJoint = new DisJoint(m * n);
        // 遍历每一行
        for (int i = 0; i < m; i++) {
            // 记录数值与列号
            Map<Integer, Integer> record = new HashMap<>();
            for (int j = 0; j < n; j++) {
                if (record.containsKey(matrix[i][j])) disJoint.union(disJoint.find(i * n + record.get(matrix[i][j])), disJoint.find(i * n + j));
                else record.put(matrix[i][j], j);
            }
        }
        // 遍历每一列
        for (int i = 0; i < n; i++) {
            // 记录数值与行号
            Map<Integer, Integer> record = new HashMap<>();
            for (int j = 0; j < m; j++) {
                if (record.containsKey(matrix[j][i])) disJoint.union(disJoint.find(record.get(matrix[j][i]) * n + i), disJoint.find(j * n + i));
                else record.put(matrix[j][i], j);
            }
        }

        int[][] res = new int[m][n];
        int[] maxRow = new int[m], maxCol = new int[n];
        for (int i = 0; i < index.length; i++) {
            int row = index[i] / n, col = index[i] % n;
            // 已经赋值
            if (res[row][col] != 0) continue;

            // 遍历同一不相交集，确定最大的秩
            int root = disJoint.find(index[i]), maxNo = 0;
            for (int j : disJoint.getIndex(root)) {
                maxNo = Math.max(maxNo, Math.max(maxRow[j / n], maxCol[j % n]) + 1);
            }
             for (int j : disJoint.getIndex(root)) {
                 res[j / n][j % n] = maxRow[j / n] = maxCol[j % n] = maxNo;
             }
        }
        return res;
    }
}

class DisJoint {
    int[] parent;
    Map<Integer, List<Integer>> index;

    DisJoint(int size) {
        this.parent = new int[size];
        this.index = new HashMap<>();
        for (int i = 0; i < size; i++) {
            parent[i] = -1;
            index.put(i, new LinkedList<>());
            index.get(i).add(i);
        }
    }

    public void union(int r1, int r2) {
        if (parent[r1] >= 0 || parent[r2] >= 0) throw new IllegalArgumentException("invalid param");
        if (r1 == r2) return;

        if (parent[r1] <= parent[r2]) {
            parent[r1] += parent[r2];
            parent[r2] = r1;
            index.get(r1).addAll(index.get(r2));
            index.remove(r2);
        } else {
            parent[r2] += parent[r1];
            parent[r1] = r2;
            index.get(r2).addAll(index.get(r1));
            index.remove(r1);
        }
    }

    public int find(int idx) {
        if (parent[idx] < 0) return idx;
        return parent[idx] = find(parent[idx]);
    }

    public List<Integer> getIndex(int root) {
        return index.get(root);
    }
}
```

## 性能分析

&emsp;最坏情况时间复杂度为$O((MN)^2)$，空间复杂度为$O(MN)$。

执行用时：432 ms, 在所有 Java 提交中击败了10.79%的用户。

内存消耗：136.2 MB, 在所有 Java 提交中击败了5.21%的用户。

## 官方解题

&emsp;暂无，密切关注。参考社区思路，在遍历过程种记录之前遍历的坐标，由于遍历的数值是有序的，故只需判断之前遍历的值是否相等，相等则获取编号，不相等则获取上次的编号并在基础上加一。

```java
class Solution {
    public int[][] matrixRankTransform(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        Integer[] index = new Integer[m * n];
        for (int i = 0; i < index.length; i++) index[i] = i;
        // 排序
        Arrays.sort(index, (a, b) -> matrix[a / n][a % n] - matrix[b / n][b % n]); 
        DisJoint disJoint = new DisJoint(m * n);
        // 记录遍历过的之前的列索引、行索引
        int[] preRow = new int[m], preCol = new int[n];
        Arrays.fill(preRow, -1);
        Arrays.fill(preCol, -1);
        // 从小到大遍历
        for (int idx : index) {
            int row = idx / n, col = idx % n;
            int no = 1;
            if (preRow[row] != -1) {
                int preIdx = row * n + preRow[row], root = disJoint.find(preIdx);
                // 当前行存在相等的值，关联
                if (matrix[row][col] == matrix[row][preRow[row]]) {
                    no = Math.max(no, disJoint.getWeight(root));
                    disJoint.union(root, row * n + col);
                }
                // 与之前遍历的不相等，则在之前的基础上编号递增
                else {
                    no = Math.max(no, disJoint.getWeight(root) + 1);
                }
            }

            if (preCol[col] != -1) {
                int preIdx = preCol[col] * n + col, root = disJoint.find(preIdx);
                // 当前列存在相等的值，关联
                if (matrix[row][col] == matrix[preCol[col]][col]) {
                    no = Math.max(no, disJoint.getWeight(root));
                    disJoint.union(root, disJoint.find(row * n + col));
                }
                // 与之前遍历的不相等，则在之前的基础上编号递增
                else {
                    no = Math.max(no, disJoint.getWeight(root) + 1);
                }
            }

            preRow[row] = col;
            preCol[col] = row;
            disJoint.updateWeight(row * n + col, no);
        }

        int[][] res = new int[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                res[i][j] = disJoint.getWeight(i * n + j);
            }
        }
        return res;
    }
}

class DisJoint {
    int[] parent;
    int[] weight;

    DisJoint(int size) {
        this.parent = new int[size];
        this.weight = new int[size];
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
    }

    public int find(int idx) {
        if (parent[idx] < 0) return idx;
        return parent[idx] = find(parent[idx]);
    }

    public int getWeight(int idx) {
        return weight[find(idx)];
    }

    public void updateWeight(int idx, int val) {
        weight[find(idx)] = val;
    }
}
```

&emsp;时间复杂度为$O(MN\log_2MN)$，空间复杂度为$O(MN)$。

执行用时：167 ms, 在所有 Java 提交中击败了54.54%的用户。

内存消耗：71.4 MB, 在所有 Java 提交中击败了49.14%的用户。