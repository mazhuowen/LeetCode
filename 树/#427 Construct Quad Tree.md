[toc]

Given a $n \times n$ matrix `grid` of `0's` and `1's` only. We want to represent the `grid` with a Quad-Tree.

Return the root of the Quad-Tree representing the `grid`.

Notice that you can assign the value of a node to **True** or **False** when isLeaf is **False**, and both are **accepted** in the answer.

A Quad-Tree is a tree data structure in which each internal node has exactly four children. Besides, each node has two attributes:

* `val`: True if the node represents a grid of 1's or False if the node represents a grid of 0's. 
* `isLeaf`: True if the node is leaf node on the tree or False if the node has the four children.

```java
class Node {
    public boolean val;
    public boolean isLeaf;
    public Node topLeft;
    public Node topRight;
    public Node bottomLeft;
    public Node bottomRight;
}
```

We can construct a Quad-Tree from a two-dimensional area using the following steps:

* If the current grid has the same value (i.e all `1's` or all `0's`) set `isLeaf` True and set `val` to the value of the grid and set the four children to Null and stop.
* If the current grid has different values, set `isLeaf` to False and set `val` to any value and divide the current grid into four sub-grids as shown in the photo.
* Recurse for each of the children with the proper sub-grid.

<img src="..\images\#427.png" style="zoom:80%;" />

If you want to know more about the Quad-Tree, you can refer to the wiki.

**Quad-Tree format**:

The output represents the serialized format of a Quad-Tree using level order traversal, where `null` signifies a path terminator where no node exists below.

It is very similar to the serialization of the binary tree. The only difference is that the node is represented as a list `[isLeaf, val]`.

If the value of `isLeaf` or `val` is True we represent it as 1 in the list `[isLeaf, val]` and if the value of `isLeaf` or `val` is False we represent it as 0.



**Constraints:**

- $n == \text{grid.length} == \text{grid[i].length}$
- $n == 2^x$ where $0 \le x \le 6$



## 题目解读

&emsp;定义四叉树节点，标识`isLeaf`表示是否是叶节点，是叶节点则`val`表示节点值，不是叶节点则有四个子节点。给定数组，构建四叉树。

```java
/*
// Definition for a QuadTree node.
class Node {
    public boolean val;
    public boolean isLeaf;
    public Node topLeft;
    public Node topRight;
    public Node bottomLeft;
    public Node bottomRight;

    
    public Node() {
        this.val = false;
        this.isLeaf = false;
        this.topLeft = null;
        this.topRight = null;
        this.bottomLeft = null;
        this.bottomRight = null;
    }
    
    public Node(boolean val, boolean isLeaf) {
        this.val = val;
        this.isLeaf = isLeaf;
        this.topLeft = null;
        this.topRight = null;
        this.bottomLeft = null;
        this.bottomRight = null;
    }
    
    public Node(boolean val, boolean isLeaf, Node topLeft, Node topRight, Node bottomLeft, Node bottomRight) {
        this.val = val;
        this.isLeaf = isLeaf;
        this.topLeft = topLeft;
        this.topRight = topRight;
        this.bottomLeft = bottomLeft;
        this.bottomRight = bottomRight;
    }
};
*/

class Solution {
    public Node construct(int[][] grid) {
        
    }
}
```

## 程序设计

* 采用分治法构建四叉树。

```java
class Solution {
    public Node construct(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length != grid.length) throw new IllegalArgumentException("invalid param");

        int n = grid.length;
        return construct(grid, 0, 0, n - 1, n -1);
    }

    private Node construct(int[][] grid, int sx, int sy, int ex, int ey) {
        if (sx == ex) return new Node(grid[sx][sy] == 1, true);

        // 分治
        int splitX = (sx + ex) / 2, splitY = (sy + ey) / 2;
        Node topLeft = construct(grid, sx, sy, splitX, splitY);
        Node topRight = construct(grid, sx, splitY + 1, splitX, ey);
        Node bottomLeft = construct(grid, splitX + 1, sy, ex, splitY);
        Node bottomRight = construct(grid, splitX + 1, splitY + 1, ex, ey);
        // 四个子节点都是叶节点且val相等，则合并为一个新的叶节点
        if (topLeft.isLeaf && topRight.isLeaf && bottomLeft.isLeaf && bottomRight.isLeaf && topLeft.val == topRight.val && topLeft.val == bottomLeft.val && topLeft.val == bottomRight.val) return new Node(topLeft.val, true);
        // 返回非叶节点
        else return new Node(true, false, topLeft, topRight, bottomLeft, bottomRight);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_4N)$。

执行用时：1ms，在所有java提交中击败了91.09%的用户。

内存消耗：40.2MB，在所有java提交中击败了69.70%的用户。

## 官方解题

&emsp;暂无，密切关注。