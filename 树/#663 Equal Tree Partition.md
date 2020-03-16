[toc]

Given a binary tree with n nodes, your task is to check if it's possible to partition the tree to two trees which have the equal sum of values after removing **exactly** one edge on the original tree.

Note:

* The range of tree node value is in the range of `[-100000, 100000]`.
* $1 \le n \le 10000$



## 题目解读

&emsp;如果树存在一条边，删除后两部分结点值之和中等，判断是否存在这条边。

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
    public boolean checkEqualTree(TreeNode root) {

    }
}
```

## 程序设计

* 首先生成结点和树，然后前序遍历判断父节点和一子节点组成的新结点集是否和另一子节点的结点和相等。

```java
class Solution {
    public boolean checkEqualTree(TreeNode root) {
        if(root == null) {
            return false;
        }
        // 生成结点和树
        TreeNode sumTree = sumTree(root);
        // 如果树的总和是奇数，不可能存在
        if(sumTree.val % 2 == 1) return false;
        // 遍历判断
        return checkSumTree(sumTree, sumTree.val);
    }

    private TreeNode sumTree(TreeNode root) {
        if(root == null) return null;
        TreeNode cur = new TreeNode();
        cur.left = sumTree(root.left);
        cur.right = sumTree(root.right);
        cur.val = (cur.left == null ? 0 : cur.left.val) + (cur.right == null ? 0 : cur.right.val) + root.val;
        return cur;
    }

    private boolean checkSumTree(TreeNode root, int allSum) {
        if(root == null) return false;
        // 子树和与剩余结点和一致，返回
        if(root.left != null && allSum == 2 * root.left.val) return true;
        if(root.right != null && allSum == 2 * root.right.val) return true;
        // 递归判断
        if(checkSumTree(root.left, allSum)) return true;
        return checkSumTree(root.right, allSum);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：41.2MB，在所有java提交中击败了11.11%的用户。

## 官方解题

&emsp;思路一致。