[toc]

We have a grid of 1s and 0s; the 1s in a cell represent bricks.  A brick will not drop if and only if it is directly connected to the top of the grid, or at least one of its (4-way) adjacent bricks will not drop.

We will do some erasures sequentially. Each time we want to do the erasure at the location `(i, j)`, the brick (if it exists) on that location will disappear, and then some other bricks may drop because of that erasure.

Return an array representing the number of bricks that will drop after each erasure in sequence.

Note:

* The number of rows and columns in the grid will be in the range `[1, 200]`.
* The number of erasures will not exceed the area of the grid.
* It is guaranteed that each erasure will be different from any other erasure, and located inside the grid.
* An erasure may refer to a location with no brick - if it does, no bricks drop.



## 题目解读

&emsp;给定矩形和砖块位置，每次取消一块砖，计算掉落的砖数目。题目中规定只有与顶部连通的砖不会掉落。

```java
class Solution {
    public int[] hitBricks(int[][] grid, int[][] hits) {

    }
}
```

## 程序设计

* 借鉴官方反向思维思考，先删除所有要消除的砖块，从时间倒序进行，每次添加本要删除的砖块，如果和当前连通到顶部的砖构成同一集合，则表明删除该砖块会导致其它砖块滑落；如果不连通则说明该砖块消除不影响其它砖块。

```java
class Solution {
    public int[] hitBricks(int[][] grid, int[][] hits) {
        if(grid == null || grid.length == 0) {
            return new int[hits.length];
        }
        int row = grid.length, col = grid[0].length;
        // 移除待删除点，标识为-1（如果本来不是砖，则不变化）
        for(int[] hit : hits) {
            if(grid[hit[0]][hit[1]] == 1) grid[hit[0]][hit[1]] = -1;
        }
        // 不相交集（设置0为连通顶部的点集合）
        DisJoint disJoint = new DisJoint(col * row + 1);
        for(int r = 0; r < row; r++) {
            for (int c = 0; c < col; c++) {
                if(grid[r][c] == 1) {
                    int idx = r * col + c + 1;
                    // 顶部元素，与边界合并
                    if(r == 0) {
                        disJoint.union(disJoint.find(0), disJoint.find(idx));
                    }
                    // 前面元素
                    if(c - 1 >= 0 && grid[r][c - 1] == 1) {
                        disJoint.union(disJoint.find(idx - 1), disJoint.find(idx));
                    }
                    // 顶部元素
                    if(r - 1 >= 0 && grid[r - 1][c] == 1) {
                        disJoint.union(disJoint.find(idx - col), disJoint.find(idx));
                    }
                }
            }
        }
        int t = hits.length - 1;
        int[] res = new int[hits.length];
        // 记录边界集合砖的数目
        int pre = disJoint.getTreeSize(0);
        while (t >= 0) {
            int r = hits[t][0], c = hits[t][1], idx = r * col + c + 1;
            // 当前点本来没有砖，不会有砖掉落
            if(grid[r][c] == 0) {
                res[t] = 0;
            } else {
                // 加入当前消除点
                grid[r][c] = 1;
                // 如果当前点是顶部砖，加入
                if(r == 0) {
                    disJoint.union(disJoint.find(0), disJoint.find(idx));
                }
                // 维护前后左右的集合
                // 前后元素
                if(c - 1 >= 0 && grid[r][c - 1] == 1) {
                    disJoint.union(disJoint.find(idx - 1), disJoint.find(idx));
                }
                if(c + 1 < col && grid[r][c + 1] == 1) {
                    disJoint.union(disJoint.find(idx + 1), disJoint.find(idx));
                }
                // 上下元素
                if(r - 1 >= 0 && grid[r - 1][c] == 1) {
                    disJoint.union(disJoint.find(idx - col), disJoint.find(idx));
                }
                if(r + 1 < row && grid[r + 1][c] == 1) {
                    disJoint.union(disJoint.find(idx + col), disJoint.find(idx));
                }

                // 如果边界集合增加了，还需要减去当前消除的结点，没有增加则是0
                res[t] = disJoint.getTreeSize(0) - pre > 0 ? disJoint.getTreeSize(0) - pre - 1 : 0;
            }
            pre = disJoint.getTreeSize(0);
            t--;
        }
        return res;
    }
}

class DisJoint {
    private int size;
    private int[] tree;

    DisJoint(int size) {
        this.size = size;
        this.tree = new int[size];
        Arrays.fill(tree, -1);
    }

    public void union(int root1, int root2) {
        if(tree[root1] >= 0 || tree[root2] >= 0) throw new IllegalArgumentException("not a root");
        if(root1 == root2) return;

        if(tree[root1] <= tree[root2]) {
            tree[root1] += tree[root2];
            tree[root2] = root1;
        } else {
            tree[root2] += tree[root1];
            tree[root1] = root2;
        }
        size--;
    }

    public int find(int idx) {
        if(tree[idx] < 0) return idx;
        return tree[idx] = find(tree[idx]);
    }

    public int size() {
        return size;
    }
    // 获取一个集合的尺寸
    public int getTreeSize(int idx) {
        return -tree[find(idx)];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\max(NM,K))$，空间复杂度为$O(NM)$，其中$N$、$M$为矩形尺寸，$K$为消除操作步数。

执行用时：7ms，在所有java提交中击败了93.24%的用户。

内存消耗：50.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;上述思路借鉴官方。