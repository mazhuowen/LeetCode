[toc]

An undirected graph of n nodes is defined by `edgeList`, where `edgeList[i] = [ui, vi, disi]` denotes an edge between nodes `ui` and `vi` with distance `disi`. Note that there may be multiple edges between two nodes.

Given an array `queries`, where `queries[j] = [pj, qj, limitj]`, your task is to determine for each `queries[j]` whether there is a path between `pj` and `qj` such that each edge on the path has a distance **strictly less than** `limitj` .

Return a **boolean array** `answer`, where `answer.length == queries.length` and the `jth` value of `answer` is `true` if there is a path for `queries[j]` is `true`, and `false` otherwise.

 

**Example 1**:

<img src="../images/#1697_exp1.png"  />

```
Input: n = 3, edgeList = [[0,1,2],[1,2,4],[2,0,8],[1,0,16]], queries = [[0,1,2],[0,2,5]]
Output: [false,true]
Explanation: The above figure shows the given graph. Note that there are two overlapping edges between 0 and 1 with distances 2 and 16.
For the first query, between 0 and 1 there is no path where each distance is less than 2, thus we return false for this query.
For the second query, there is a path (0 -> 1 -> 2) of two edges with distances less than 5, thus we return true for this query.
```

**Example 2**:

<img src="../images/#1697_exp2.png"  />

```
Input: n = 5, edgeList = [[0,1,10],[1,2,5],[2,3,9],[3,4,13]], queries = [[0,4,14],[1,4,13]]
Output: [true,false]
Exaplanation: The above figure shows the given graph.
```



**Constraints**:

* $2 \le n \le 10^5$
* $1 \le \text{edgeList.length}, \text{queries.length} \le 10^5$
* $\text{edgeList[i].length} == 3$
* $\text{queries[j].length} == 3$
* $0 \le u_i, v_i, p_j, q_j \le n - 1$
* $u_i \ne v_i$
* $p_j \ne q_j$
* $1 \le \text{dis}_i, \text{limit}_j \le 10^9$
* There may be **multiple** edges between two nodes.



## 题目解读

&emsp;给定图及查询，求查询两点间是否存在每条边的长度严格小于限定值的路径。

```java
class Solution {
    public boolean[] distanceLimitedPathsExist(int n, int[][] edgeList, int[][] queries) {

    }
}
```

## 程序设计

* 通过将边按权重排序，查询也按照限定排序，依次加入不相交集来判断。

```java
class Solution {
    public boolean[] distanceLimitedPathsExist(int n, int[][] edgeList, int[][] queries) {
        // 根据路径权重排序
        Arrays.sort(edgeList, (a, b) -> a[2] - b[2]);
        // 根据限定值大小排序查询索引
        Integer[] index = new Integer[queries.length];
        for (int i = 0; i < queries.length; i++) index[i] = i;
        Arrays.sort(index, (a, b) -> queries[a][2] - queries[b][2]);
        
        DisJoint disJoint = new DisJoint(n);
        boolean[] res = new boolean[queries.length];
        int idx = 0;
        for (int i = 0; i < queries.length; i++) {
            int[] query = queries[index[i]];
            // 将当前小于限定值的边加入不相交集
            while (idx < edgeList.length && edgeList[idx][2] < query[2]) {
                disJoint.union(disJoint.find(edgeList[idx][0]), disJoint.find(edgeList[idx][1]));
                idx++;
            }
            // 查询是否连通
            res[index[i]] = disJoint.find(query[0]) == disJoint.find(query[1]);
        }
        return res;
    }
}

class DisJoint {
    int[] parent;
    
    DisJoint(int size) {
        this.parent = new int[size];
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
        return parent[idx] < 0 ? idx : (parent[idx] = find(parent[idx]));
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log N + M\log M)$，空间复杂度为$O(N + M)$。

执行用时：126 ms, 在所有 Java 提交中击败了32.25%的用户。

内存消耗：66.9 MB, 在所有 Java 提交中击败了76.87%的用户。

## 官方解题

&emsp;暂无，密切关注。
