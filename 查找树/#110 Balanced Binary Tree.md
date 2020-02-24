[toc]

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

a binary tree in which the left and right subtrees of every node differ in height by no more than 1.

 

## 题目解读

&emsp;给定二叉树，判断是否高度平衡。

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
    public boolean isBalanced(TreeNode root) {

    }
}
```

## 程序设计

* 首先想到的是使用递归计算结点高度，通过比对高度差，设置外面的全局变量。

```java
class Solution {
    private boolean flag = true;
    public boolean isBalanced(TreeNode root) {
        getHight(root);
        return flag;
    }

    private int getHight(TreeNode root) {
        if(root == null) {
            return 0;
        }
        int left = getHight(root.left);
        int right = getHight(root.right);
        // 不满足条件
        if(Math.abs(left - right) > 1) {
            flag = false;
        }
        return Math.max(left, right) + 1;
    }
}
```

## 性能分析

&emsp;递归时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：1ms，在所有java提交中击败了99.90%的用户。

内存消耗：41.5MB，在所有java提交中击败了8.77%的用户。

## 官方解题

&emsp;同上。