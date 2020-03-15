[toc]

On a 2D plane, we place stones at some integer coordinate points.  Each coordinate point may have at most one stone.

Now, a move consists of removing a stone that shares a column or row with another stone on the grid.

What is the largest possible number of moves we can make?

**Note:**

* $1 \le \text{stones.length} \le 1000$
* $0 \le \text{stones[i][j]} < 10000$



## 题目解读

&emsp;移除在同一行或同一列的结点，返回可以移除的最大数目。

```java
class Solution {
    public int removeStones(int[][] stones) {

    }
}
```

## 程序设计

* 仔细观察发现，对于横坐标或纵坐标传递的集合，都可以消去只剩最后一个点。这样问题化为通过横坐标、纵坐标扩展集合，最后剩余的最少结点数就是集合数，可以删除的最大结点数就是总的减去最少剩余结点数。

```java
class Solution {
    public int removeStones(int[][] stones) {
        if(stones == null || stones.length <= 1) {
            return 0;
        }
        // 根据坐标合并
        DisJoint disJoint = new DisJoint(stones.length);
        for(int i = 0; i < stones.length - 1; i++) {
            for(int j = i + 1; j < stones.length; j++) {
                // 如果横坐标或纵坐标相等，则合并
                if(stones[i][0] == stones[j][0] || stones[i][1] == stones[j][1]) {
                    disJoint.union(disJoint.find(i), disJoint.find(j));
                }
            }
        }
        // 最后删除后剩余集合个结点，删除的结点数就是总的减去剩余的
        return stones.length - disJoint.size();
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
    // 按规模并
    public void union(int root1, int root2) {
        if(tree[root1] >= 0 || tree[root2] >= 0) {
            throw new IllegalArgumentException("not a root");
        }
        if(root1 == root2) {
            return;
        }
        // 树2大，树1并入树2
        if(tree[root1] >= tree[root2]) {
            tree[root2] += tree[root1];
            tree[root1] = root2;
        } else {
            tree[root1] += tree[root2];
            tree[root2] = root1;
        }
        size--;
    }
    // 路径压缩
    public int find(int val) {
        if(tree[val] < 0) return val;
        return tree[val] = find(tree[val]);
    }

    public int size() {
        return size;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：62ms，在所有java提交中击败了9.60%的用户。

内存消耗：40.7MB，在所有java提交中击败了76.19%的用户。

## 官方解题

&emsp;官方解题巧妙利用规则，将同一个点的横坐标、纵坐标合并，从而只需遍历一次。

```java
class Solution {
    public int removeStones(int[][] stones) {
        if(stones == null || stones.length <= 1) {
            return 0;
        }
        // 根据坐标合并
        DisJoint disJoint = new DisJoint(20000);
        for(int i = 0; i < stones.length; i++) {
            disJoint.union(disJoint.find(stones[i][0]), disJoint.find(stones[i][1] + 10000));
        }
        // 最后删除后剩余集合个结点，删除的结点数就是总的减去剩余的
        Set<Integer> set = new HashSet<>();
        for(int i = 0; i < stones.length; i++) {
            set.add(disJoint.find(stones[i][0]));
        }
        return stones.length - set.size();
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：14ms，在所有java提交中击败了45.60%的用户。

内存消耗：41.7MB，在所有java提交中击败了61.90%的用户。