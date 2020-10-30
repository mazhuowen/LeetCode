[toc]

An undirected, connected tree with `N` nodes labelled $0 \dots N-1$ and $N-1$ `edges` are given.

The `i`th edge connects nodes `edges[i][0]` and `edges[i][1]` together.

Return a list `ans`, where `ans[i]` is the sum of the distances between node `i` and all other nodes.

Note: $1 \le N \le 10000$



## 题目解读

&emsp;给定边的数组表示无向树，返回每个结点到其它结点的距离之和。

```java
class Solution {
    public int[] sumOfDistancesInTree(int N, int[][] edges) {

    }
}
```

## 程序设计

* 实质题目中的无向树就是无环拓扑图，为了计算每个结点到其它结点的距离，可以使用动态规划，每次更新并延长路径1，对于同父节点则更新为2。但该算法要求数组的第一个结点是父结点，第二个结点是子节点，不符合题目测试用例。

```java
class Solution {
    public int[] sumOfDistancesInTree(int N, int[][] edges) {
        // 动态规划
        int[][] dp = new int[N][N];
        for(int i = 0; i < N - 1; i++) {
            // 父节点
            int node1 = edges[i][0];
            // 子节点
            int node2 = edges[i][1];
            // 赋值（由于无向，两边都赋值）
            dp[node1][node2] = dp[node2][node1] = 1;
            // 更新node1结尾的路径，与node1～node2合并
            for(int row = 0; row < N; row++) {
                // 等于0表示两点间暂时是断开的或是同一个点，不予更新
                // 其次需要排序node1～node2与node2～node1这种循环更新node1～node1的情况
                if(dp[row][node1] != 0 && row != node2) {
                    dp[row][node2] = dp[node2][row] = dp[row][node1] + 1;
                }
            }

            // 更新同父结点
            for(int col = 0; col < N; col++) {
                // 和node1距离为1的也有可能是祖父结点而不是兄弟节点，需要判别
                // 当node1与col是父子结点时，col～node2必然是有值的，因为上面更新过，故dp[node1][col] == 1且dp[col][node2] == 0表示的就是同父节点，更新为2
                if(dp[node1][col] == 1 && col != node2 && dp[col][node2] == 0) {
                    dp[node2][col] = dp[col][node2] = 2;
                }
            }
        }
        
        int[] res = new int[N];
        for(int i = 0; i < N; i++) {
            for(int j = 0; j < N; j++) {
                res[i] += dp[i][j];
            }
        }
        return res;
    }
}
```

* 参考官方思路，以某个结点为根，统计各个子树的路径和，根的路径和可以根据子树路径和和结点数得到；子树与除了子树之外结点间的路径和又可以可以根节点得到。通过指定一个结点为根，将无向图转化为有向树，通过根与子树之间的关系，动态规划的得到所有结点的路径和。

```java
class Solution {
    public int[] sumOfDistancesInTree(int N, int[][] edges) {
        // 记录距离和
        int[] res = new int[N];
        // 记录子树的结点数
        int[] count = new int[N];
        Arrays.fill(count, 1);
        // 记录子节点（由于是无向图，记录邻接结点）
        List<Set<Integer>> graph = new ArrayList<>();
        for(int i = 0; i < N; i++) {
            graph.add(new HashSet<>());
        }
        // 加入邻接结点（根据结点编号）
        for(int[] edge : edges) {
            graph.get(edge[0]).add(edge[1]);
            graph.get(edge[1]).add(edge[0]);
        }
        // 以结点0为根节点，统计子树结点和路径和（自底向上）
        countSubTree(0, -1, graph, res, count);
        // 更新子树与子树之间的路径和（自顶向下）
        countTreeByTree(0, -1, graph, res, count);
        return res;
    }

    // 统计更新子树的结点数和子树根到各节点的路径和
    private void countSubTree(int cur, int parent, List<Set<Integer>> graph, int[] res, int[] count) {
        Set<Integer> children = graph.get(cur);
        for(int child : children) {
            // 排除邻接结点包含父节点，避免回去
            if(child == parent) continue;
            countSubTree(child, cur, graph, res, count);
            // 更新以父节点为根的子树结点值
            count[cur] += count[child];
            // 更新父节点到child子树的各节点路径和
            // 子节点child为根的子树根到各个结点的路径和为res[child]，则cur到这个子树的路径和就是子树节点数个cur到child的距离，即1
            res[cur] += res[child] + count[child];
        }
    }
    
    // 更新子树与子树之间的路径和
    private void countTreeByTree(int cur, int parent, List<Set<Integer>> graph, int[] res, int[] count) {
        Set<Integer> children = graph.get(cur);
        // 更新子树间的路径和
        for(int child : children) {
            if(child == parent) continue;
            // 除去child的根节点和其它子树组成的子树的路径和为：
            int sum = res[cur] - res[child] - count[child];
            // 除去child的节点数
            int num = res.length - count[child];
            // 更新child到其它子树的路径和
            res[child] += sum + num;
            countTreeByTree(child, cur, graph, res, count);
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：33ms，在所有java提交中击败了73.15%的用户。

内存消耗：54MB，在所有java提交中击败了62.69%的用户。

## 官方解题

&emsp;参考官方解题。

