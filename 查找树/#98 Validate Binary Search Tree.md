[toc]

Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

* The left subtree of a node contains only nodes with keys less than the node's key.
* The right subtree of a node contains only nodes with keys greater than the node's key.
* Both the left and right subtrees must also be binary search trees.



## 题目解读

&emsp;判断给定的二叉树是否是二叉查找树。

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
    public boolean isValidBST(TreeNode root) {

    }
}
```

## 程序设计

* 首先可以使用递归检测：

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValidBST(root, null, null);
    }

    private boolean isValidBST(TreeNode root, Integer low, Integer high) {
        // 递归终止条件
        if(root == null) {
            return true;
        }
        if(low != null && low >= root.val) {
            return false;
        }
        if(high != null && high <= root.val) {
            return false;
        }
        return isValidBST(root.left, low, root.val) && isValidBST(root.right, root.val, high);
    }
}
```

* 联想到二叉查找树的重要性质（充要条件），中序遍历是递增的，可以使用中序遍历来检测。注：也可以使用迭代方式实现，此处不再累述。

```java
class Solution {
    // 保存前驱
    private Integer pre = null;

    public boolean isValidBST(TreeNode root) {
        // 递归终止条件
        if(root == null) {
            return true;
        }
        if(!isValidBST(root.left)) {
            return false;
        }
        // 检查前驱并赋值
        if(pre != null && pre >= root.val) {
            return false;
        }
        pre = root.val;
        if(!isValidBST(root.right)) {
            return false;
        }
        return true;
    }
}
```

## 性能分析

&emsp;递归方法时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：41MB，在所有java提交中击败了5.01%的用户。

&emsp;中序遍历方法时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：1ms，在所有java提交中击败了82.38%的用户。

内存消耗：39.8MB，在所有java提交中击败了5.01%的用户。

## 官方解题

&emsp;同上。