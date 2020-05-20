[toc]

Given the root of a binary **search** tree with distinct values, modify it so that every `node` has a new value equal to the sum of the values of the original tree that are greater than or equal to `node.val`.

As a reminder, a binary search tree is a tree that satisfies these constraints:

* The left subtree of a node contains only nodes with keys **less than** the node's key.
* The right subtree of a node contains only nodes with keys **greater than** the node's key.
* Both the left and right subtrees must also be binary search trees.



Constraints:

* The number of nodes in the tree is between `1` and `100`.
* Each node will have value between `0` and `100`.
* The given tree is a binary search tree.



## 题目解读

&emsp;将二叉查找树转化为最大和树。同[#538 Convert BST to Greater Tree](./#538 Convert BST to Greater Tree.md)。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode bstToGst(TreeNode root) {

    }
}
```

## 程序设计

* 中序遍历，先遍历右节点，再遍历左节点。

```java
class Solution {
    // 记录后面的数字和
    int next = 0;

    public TreeNode bstToGst(TreeNode root) {
        if (root == null) return null;
        bstToGst(root.right);
        // 将后面的数字和加入当前节点
        root.val += next;
        // 更新数字和
        next = root.val;
        bstToGst(root.left);
        return root;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.5MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。