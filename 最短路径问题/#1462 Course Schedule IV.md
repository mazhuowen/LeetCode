[toc]

There are a total of $n$ courses you have to take, labeled from $0$ to $n-1$.

Some courses may have direct prerequisites, for example, to take course $0$ you have first to take course $1$, which is expressed as a pair: `[1,0]`

Given the total number of courses $n$, a list of direct `prerequisite` pairs and a list of `queries` pairs.

You should answer for each `queries[i]` whether the course `queries[i][0]` is a prerequisite of the course `queries[i][1]` or not.

Return a list of boolean, the answers to the given `queries`.

Please note that if course a is a prerequisite of course b and course b is a prerequisite of course c, then, course a is a prerequisite of course c.

 

**Constraints**:

* $2 \le n \le 100$
* $0 \le \text{prerequisite.length} \le (n * (n - 1) / 2)$
* $0 \le \text{prerequisite[i][0], prerequisite[i][1]} < n$
* $\text{prerequisite[i][0]} \ne \text{prerequisite[i][1]}$
* The prerequisites graph has no cycles.
* The prerequisites graph has no repeated edges.
* $1 \le \text{queries.length} \le 10^4$
* $\text{queries[i][0]} \ne \text{queries[i][1]}$



## 题目解读

&emsp;给定课程先修列表，判断查询列表每一对是否是先修关系。

```java
class Solution {
    public List<Boolean> checkIfPrerequisite(int n, int[][] prerequisites, int[][] queries) {
        
    }
}
```

## 程序设计

* 粗看题目似乎是拓扑排序问题，但是需要计算查询给定查询对间的依赖传递关系；由于题目限定无环无重复边，事实上必然存在拓扑排序，只要路径可达则表示路径上的子节点依赖祖先节点，这样问题转化为顶点对距离问题，可修改`Floyd`算法如下：

```java
class Solution {
    public List<Boolean> checkIfPrerequisite(int n, int[][] prerequisites, int[][] queries) {
        // 初始化邻接矩阵
        boolean[][] distance = new boolean[n][n];
        for (int[] request : prerequisites) {
            int to = request[0], from = request[1];
            distance[from][to] = true;
        }

        // Floyd算法
        for (int k = 0; k < n; k++) {
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++) {
                    if (distance[i][k] && distance[k][j]) distance[i][j] = true;
                }
            }
        }

        // 查询拼接
        List<Boolean> res = new LinkedList<>();
        for (int[] query : queries) {
            res.add(distance[query[1]][query[0]]);
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^3)$，空间复杂度为$O(N^2)$。

执行用时：30ms，在所有java提交中击败了84.11%的用户。

内存消耗：43MB，在所有java提交中击败了90.32%的用户。

## 官方解题

&emsp;暂无，密切关注。