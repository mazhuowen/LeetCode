[toc]

Given two binary trees, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical and the nodes have the same value.



## 题目解读

&emsp;给定两个二叉树，比较树的结构、数值是否一致。涉及二叉树的遍历比较。

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
    public boolean isSameTree(TreeNode p, TreeNode q) {
      
    }
}
```

## 程序设计

* 通过遍历实现，可以是中序遍历、前序遍历、后序遍历、层次遍历。

```java
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        // 递归终止条件
        if(p == null || q == null) {
            return p == q;
        }
        // 递归左子树
        if(!isSameTree(p.left, q.left)) {
            return false;
        }
        // 比较当前结点
        if(p.val != q.val) {
            return false;
        }
        // 递归右子树
        if(!isSameTree(p.right, q.right)) {
            return false;
        }
        return true;
    }
}
```

## 性能分析

&emsp;递归时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏情况树退化为链表，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：34.1MB，在所有java提交中击败了65.63%的用户。

## 官方解题

&emsp;除了递归，官方还提供了非递归实现，即两个二叉树同时层次遍历比较。