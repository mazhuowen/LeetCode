[toc]

You are asked to cut off trees in a forest for a golf event. The forest is represented as a non-negative 2D map, in this map:

* `0` represents the obstacle can't be reached.
* `1` represents the ground can be walked through.
* `The place with number bigger than 1` represents a `tree` can be walked through, and this positive number represents the tree's height.

In one step you can walk in any of the four directions `top`, `bottom`, `left` and `right` also when standing in a point which is a tree you can decide whether or not to cut off the tree.

You are asked to cut off **all** the trees in this forest in the order of tree's height - always cut off the tree with lowest height first. And after cutting, the original place has the tree will become a grass (value 1).

You will start from the point (0, 0) and you should output the minimum steps **you need to walk** to cut off all the trees. If you can't cut off all the trees, output -1 in that situation.

You are guaranteed that no two `trees` have the same height and there is at least one tree needs to be cut off.



Constraints:

* $1 \le \text{forest.length} \le 50$
* $1 \le \text{forest[i].length} \le 50$
* $0 \le \text{forest[i][j]} \le 10^9$



## 题目解读

&emsp;求砍掉所有树所需的最少步数，要求从低到高砍树，不能穿过障碍物，树可以走过。

```java
class Solution {
    public int cutOffTree(List<List<Integer>> forest) {

    }
}
```

## 程序设计

* 参考官方解题，由于需要按照树的高度从低到高砍树，首先将树的高度排序，然后从起点开始计算到树的最短距离，计算距离使用广度优先算法。
* 对于高度相同的树，砍树的顺序部分先后，可以证明不论先砍哪棵树，距离都是相等的（如果连通的话）。

```java
class Solution {
    int m, n;
    int[] delta = new int[]{0, 1, 0, -1, 0};

    public int cutOffTree(List<List<Integer>> forest) {
        if (forest == null || forest.isEmpty()) throw new IllegalArgumentException("invalid param");

        m = forest.size();
        n = forest.get(0).size();
        List<int[]> trees = new ArrayList<>();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int high = forest.get(i).get(j);
                // 是树，加入
                if (high > 1) trees.add(new int[]{i, j, high});
            }
        }
        // 根据树的高度排序
        Collections.sort(trees, (a, b) -> a[2] - b[2]);

        int dis = 0;
        // 从高到底砍树
        int sr = 0, sc = 0;
        for (int i = 0; i < trees.size(); i++) {
            int curDis = bfs(forest, sr, sc, trees.get(i)[0], trees.get(i)[1]);
            // 无法到达
            if (curDis == -1) return -1;

            dis += curDis;
            sr = trees.get(i)[0];
            sc = trees.get(i)[1];
        }
        return dis;
    }

    // 广度优先搜索求解最短距离
    private int bfs(List<List<Integer>> forest, int sr, int sc, int tr, int tc) {
        // 记录遍历过的点
        Set<Integer> record = new HashSet<>();
        Queue<int[]> queue = new LinkedList<>();
        // 加入起始点
        queue.add(new int[]{sr, sc});
        record.add(sr * n + sc);

        int dis = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int[] cur = queue.poll();
                // 到达目标点
                if (cur[0] == tr && cur[1] == tc) return dis;

                for (int j = 0; j < 4; j++) {
                    int x = cur[0] + delta[j], y = cur[1] + delta[j + 1];
                    // 坐标不符合或是障碍物或已遍历
                    if (x < 0 || x >= m || y < 0 || y >= n
                        || forest.get(x).get(y) <= 0 || record.contains(x * n + y)) continue;
                    record.add(x * n + y);
                    queue.add(new int[]{x, y});
                }
            }
            dis++;
        }
        // 无法到达
        return -1;
    }
}
```

> 记录遍历过的节点可使用数组，时间性能更好。

## 性能分析

&emsp;时间复杂度为$O(M^2N^2)$，空间复杂度为$O(MN)$。

执行用时：788ms，在所有java提交中击败了13.48%的用户。

内存消耗：40.3MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;上述思路参考官方解题。