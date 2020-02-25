[toc]

Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both `p` and `q` as descendants (where we allow a node to be a descendant of itself).”



Note:

* All of the nodes' values will be unique.
* `p` and `q` are different and both values will exist in the BST.



## 题目解读

&emsp;给定二叉查找树和两个值，给出最近公共祖先。

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

* 可以像二叉树那样递归或迭代记录父节点然后比较得出。但是考虑到二叉查找树的性质，如果当前结点大于或小于两个值，则两个值在同一侧，继续迭代；如果当前结点等于两个值中的一个，则就是LCA；如果当前结点在两个值中间，则当前结点就是LCA；实质是二叉查找树搜索的变形，采用递归即可。

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null) {
            return null;
        }
        // 当前值在这个结点中间，就是LCA
        if((p.val < root.val && q.val > root.val) || (p.val > root.val && q.val < root.val)) {
            return root;
        } 
        // 当前值是两个结点中的一个，就是LCA
        else if(p.val == root.val || q.val == root.val) {
            return root;
        } else {
            // 向左搜索
            if(p.val < root.val) {
                return lowestCommonAncestor(root.left, p, q);
            } 
            // 向右搜索
            else {
                return lowestCommonAncestor(root.right, p, q);
            }
        }
    }
}
```

* 迭代实现：

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        while(root != null) {
            if((root.val > p.val && root.val < q.val) || (root.val < p.val && root.val > q.val)) {
                return root;
            } else if(root.val == q.val || root.val == p.val) {
                return root;
            } else if(root.val > q.val) {
                root = root.left;
            } else {
                root = root.right;
            }
        }
        return null;
    }
}
```

## 性能分析

&emsp;递归方法时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：6ms，在所有java提交中击败了99.73%的用户。

内存消耗：42.2MB，在所有java提交中击败了5.03%的用户。

&emsp;迭代方法时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：7ms，在所有java提交中击败了81.27%的用户。

内存消耗：42.4MB，在所有java提交中击败了5.03%的用户。

## 官方解题

&emsp;同上。