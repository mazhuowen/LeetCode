[toc]

A Binary Matrix is a matrix in which all the elements are either **0** or **1**.

Given `quadTree1` and `quadTree2`. `quadTree1` represents a $n \times n$ binary matrix and `quadTree2` represents another $n \times n$ binary matrix. 

Return a Quad-Tree representing the $n \times n$ binary matrix which is the result of **logical bitwise OR** of the two binary matrixes represented by `quadTree1` and `quadTree2`.

Notice that you can assign the value of a node to **True** or **False** when `isLeaf` is **False**, and both are **accepted** in the answer.

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

<img src="..\images\#558.png"  />

If you want to know more about the Quad-Tree, you can refer to the wiki.

**Quad-Tree format**:

The input/output represents the serialized format of a Quad-Tree using level order traversal, where `null` signifies a path terminator where no node exists below.

It is very similar to the serialization of the binary tree. The only difference is that the node is represented as a list `[isLeaf, val]`.

If the value of `isLeaf` or `val` is True we represent it as **1** in the list `[isLeaf, val]` and if the value of `isLeaf` or `val` is False we represent it as **0**.



**Constraints**:

* `quadTree1` and `quadTree2` are both **valid** Quad-Trees each representing a $n \times n$ grid.
* $n == 2^x$ where $0 \le x \le 9$.



## 题目解读

&emsp;四叉树表示二进制矩阵，求两个矩阵的或运算。

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

    public Node() {}

    public Node(boolean _val,boolean _isLeaf,Node _topLeft,Node _topRight,Node _bottomLeft,Node _bottomRight) {
        val = _val;
        isLeaf = _isLeaf;
        topLeft = _topLeft;
        topRight = _topRight;
        bottomLeft = _bottomLeft;
        bottomRight = _bottomRight;
    }
};
*/

class Solution {
    public Node intersect(Node quadTree1, Node quadTree2) {
        
    }
}
```

## 程序设计

* 四叉树遍历，需要理解二进制矩阵的或运算规则，当当前节点有一个是叶子节点，若叶子节点值是$1$，则或运算也是$1$，无需遍历；若是$0$，则只需返回另一个节点。当当前节点都不是叶子节点，则递归计算，需注意要判断子节点值是否一致，一致则合并为叶子节点。

```java
class Solution {
    public Node intersect(Node quadTree1, Node quadTree2) {
        // 存在至少一个叶子节点
        if (quadTree1.isLeaf || quadTree2.isLeaf) {
            return quadTree1.isLeaf ? (quadTree1.val ? quadTree1 : quadTree2) : (quadTree2.val ? quadTree2 : quadTree1);
        }
        // 不是叶子节点
        else {
            Node topLeft = intersect(quadTree1.topLeft, quadTree2.topLeft);
            Node topRight = intersect(quadTree1.topRight, quadTree2.topRight);
            Node bottomLeft = intersect(quadTree1.bottomLeft, quadTree2.bottomLeft);
            Node bottomRight = intersect(quadTree1.bottomRight, quadTree2.bottomRight);

            boolean bothLeaf = topLeft.isLeaf && topRight.isLeaf && bottomLeft.isLeaf && bottomRight.isLeaf;
            boolean bothOne = bothLeaf && topLeft.val && topRight.val && bottomLeft.val && bottomRight.val;
            boolean bothZero = bothLeaf && !topLeft.val && !topRight.val && !bottomLeft.val && !bottomRight.val;
            
            // 如果子区间值相等，则设置为叶子节点返回
            if (bothOne || bothZero) return new Node(bothOne, true, null, null, null, null);
            else return new Node(false, false, topLeft, topRight, bottomLeft, bottomRight);  
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_4N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：40MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。