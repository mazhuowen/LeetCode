[toc]

There is a strange printer with the following two special requirements:

* On each turn, the printer will print a solid rectangular pattern of a single color on the grid. This will cover up the existing colors in the rectangle.
* Once the printer has used a color for the above operation, **the same color cannot be used again**.

You are given a $m \times n$ matrix `targetGrid`, where `targetGrid[row][col]` is the color in the position `(row, col)` of the grid.

Return `true` if it is possible to print the matrix `targetGrid`, otherwise, return `false`.



**Constraints**:

* $m == \text{targetGrid.length}$
* $n == \text{targetGrid[i].length}$
* $1 \le m, n \le 60$
* $1 \le \text{targetGrid[row][col]} \le 60$



## 题目解读

&emsp;给定打印机，每次只能用同种颜色打印一块矩形区域，这次打印的颜色后续不能再次打印，判断是否可以得到给定的矩形着色方案。

```java
class Solution {
    public boolean isPrintable(int[][] targetGrid) {

    }
}
```

## 程序设计

* 由于涂色有先后顺序，且每种颜色只能涂一次，可将涂色过程看作是拓扑排序，颜色为节点；
* 问题关键在于颜色之间依赖关系的判断，考虑到最后涂色的矩形必然是纯色构成，而非最后涂色的矩形其内部存在其他颜色，即其他颜色依赖当前颜色；
* 对于每种颜色矩形的区域判断可以直接遍历判断，取四个坐标的最大值。

```java
class Solution {
    // 颜色，即节点总数
    private final static int COLOR = 61;

    public boolean isPrintable(int[][] targetGrid) {
        int n = targetGrid.length, m = targetGrid[0].length;
        // 每种颜色的矩形区域，记录四个顶点坐标
        int[] minRow = new int[COLOR];
        int[] maxRow = new int[COLOR];
        int[] minCol = new int[COLOR];
        int[] maxCol = new int[COLOR];
        Arrays.fill(minRow, Integer.MAX_VALUE);
        Arrays.fill(minCol, Integer.MAX_VALUE);
        for (int i = 0; i < n; i++) {
            int[] row = targetGrid[i];
            for (int j = 0; j < m; j++) {
                int col = row[j];
                // 上下行，左右列坐标
                minRow[col] = Math.min(minRow[col], i);
                maxRow[col] = Math.max(maxRow[col], i);
                minCol[col] = Math.min(minCol[col], j);
                maxCol[col] = Math.max(maxCol[col], j);
            }
        }

        // 构图
        int[] inDegree = new int[COLOR];
        Node[] graph = new Node[COLOR];
        // 判断是否已经连接，避免重复连接
        boolean[][] visited = new boolean[COLOR][COLOR];
        for (int col = 0; col < COLOR; col++) {
            // 不存在该颜色的区域
            if (minRow[col] == Integer.MAX_VALUE) continue;
            // 遍历判断区域是否有其它颜色
            for (int i = minRow[col]; i <= maxRow[col]; i++) {
                for (int j = minCol[col]; j <= maxCol[col]; j++) {
                    // 颜色一致或已经加入边，则跳过
                    if (col == targetGrid[i][j] || visited[targetGrid[i][j]][col]) continue;
                    graph[targetGrid[i][j]] = new Node(col, graph[targetGrid[i][j]]);
                    inDegree[col]++;
                }
            }
        }

        // 拓扑排序
        Queue<Integer> queue = new LinkedList<>();
        // 入度非0的节点数目
        int num = 0;
        for (int i = 0; i < COLOR; i++) {
            if (inDegree[i] == 0) queue.add(i);
            else num++;
        }

        while (!queue.isEmpty()) {
            int cur = queue.poll();
            for (Node tmp = graph[cur]; tmp != null; tmp = tmp.next) {
                if (--inDegree[tmp.ver] == 0) {
                    queue.add(tmp.ver);
                    num--;
                }
            }
        }
        // 判断是否存在环
        return num == 0;
    }
}

class Node {
    int ver;
    Node next;

    Node(int ver, Node next) {
        this.ver = ver;
        this.next = next;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\min(N,M)NM)$，空间复杂度为$O(NM)$。

执行用时：25ms，在所有java提交中击败了50.00%的用户。

内存消耗：48.1MB，在所有java提交中击败了6.67%的用户。

## 官方解题

&emsp;暂无，密切关注。