[toc]

Given preorder and inorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.



## 题目解读

&emsp;给定前序遍历和中序遍历，得到二叉树。

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
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        
    }
}
```

## 程序设计

* 根据前序遍历的特点，首个值就是头结点；根据中序遍历特点，头结点左边就是左子树序列，右边就是右子树序列。可使用递归的方法。

```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if(preorder == null || inorder == null) {
            return null;
        }
        return buildTree(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1);
    }

    private TreeNode buildTree(int[] preorder, int start1, int end1, int[] inorder, int start2, int end2) {
        if(start1 > end1 || start2 > end2) {
            return null;
        }
        // 头结点为前序遍历的第一个值
        TreeNode head = new TreeNode(preorder[start1]);
        int index = start2;
        // 从中序遍历中找到头结点
        for(int i = start2; i <= end2; i++) {
            if(inorder[i] == preorder[start1]) {
                index = i;
                break;
            }
        }
        // 左子树前序序列从start+1开始
        head.left = buildTree(preorder, start1 + 1, start1 + index - start2, inorder, start2, index - 1);
        // 右子树前序遍历从start加左子树数目加1开始，此处左子树结点数就是index-start2
        head.right = buildTree(preorder, start1 + index - start2 + 1, end1, inorder, index + 1, end2);
        return head;
    }
}
```

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度为$O(\log_2N)$，最坏情况为$O(N)$。

执行用时：14ms，在所有java提交中击败了46.46%的用户。

内存消耗：43.5MB，在所有java提交中击败了11.17%的用户。

## 官方解题

&emsp;暂无，密切关注。

