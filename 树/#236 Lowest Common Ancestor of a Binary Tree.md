[toc]

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the `definition of LCA on Wikipedia`: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow **a node to be a descendant of itself**).”



Note:

* All of the nodes' values will be unique.
* p and q are different and both values will exist in the binary tree.



## 题目解读

&emsp;查找两个结点的共同祖先。

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
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        
    }
}
```

## 程序设计

* 查找共同的祖先就是自底到顶遍历父结点的过程，可以使用递归实现。



## 性能分析



## 官方解题

