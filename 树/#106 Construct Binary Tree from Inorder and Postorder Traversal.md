[toc]

Given inorder and postorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.



## 题目解读

&emsp;给定中序遍历、后序遍历，构造二叉树。

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
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        
    }
}
```

## 程序设计

* 头结点在后序遍历的最后一位，中序遍历头结点左边为左子树序列，右边为右子树序列。

```java
class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        if(inorder == null || postorder == null) {
            return null;
        }
        return buildTree(inorder, 0, inorder.length - 1, postorder, 0, postorder.length - 1);
    }

    private TreeNode buildTree(int[] inorder, int start1, int end1, int[] postorder, int start2, int end2) {
        if(start1 > end1 || start2 > end2) {
            return null;
        }
        // 头结点
        TreeNode  head = new TreeNode(postorder[end2]);
        // 找到中序遍历中的头结点
        int index = start1;
        for(int i = start1; i <= end1; i++) {
            if(inorder[i] == postorder[end2]) {
                index = i;
                break;
            }
        }
        // 递归
        head.left = buildTree(inorder, start1, index - 1, postorder, start2, end2 - end1 + index - 1);
        head.right = buildTree(inorder, index + 1, end1, postorder, end2 - end1 + index, end2 - 1);
        return head;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏为$O(N)$。

执行用时：14ms，在所有java提交中击败了42.16%的用户。

内存消耗：42.8MB，在所有java提交中击败了18.10%的用户。

## 官方解题

&emsp;暂无，密切关注。