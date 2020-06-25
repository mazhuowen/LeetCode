[toc]

Return the root node of a binary **search** tree that matches the given `preorder` traversal.

(Recall that a binary search tree is a binary tree where for every `node`, any descendant of `node.left` has a value `< node.val`, and any descendant of `node.right` has a value `> node.val`.  Also recall that a preorder traversal displays the value of the `node` first, then traverses `node.left`, then traverses `node.right`.)

It's guaranteed that for the given test cases there is always possible to find a binary search tree with the given requirements.



**Constraints**:

* $1 \le \text{preorder.length} \le 100$
* $1 \le \text{preorder[i]} \le 10^8$
* The values of `preorder` are distinct.



## 题目解读

&emsp;根据前序序列构建二叉搜索树。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode bstFromPreorder(int[] preorder) {
        
    }
}
```

## 程序设计

* 由于是二叉查找树的前序序列，故数组第一个值是根节点，其后第一个值若小于根节点，则为左子树根节点，其后若存在大于根节点的值，则为右子树根节点。

```java
class Solution {
    public TreeNode bstFromPreorder(int[] preorder) {
        if (preorder == null || preorder.length == 0) return null;
        return bstFromPreorder(preorder, 0, preorder.length - 1);
    }

    private TreeNode bstFromPreorder(int[] preorder, int start, int end) {
        if (start > end) return null;

        TreeNode root = new TreeNode(preorder[start]);

        // 右子树根节点为数组中第一个大于当前节点的节点
        int right = start + 1;
        while (right <= end && preorder[right] < preorder[start]) right++;
        // 递归
        root.left = bstFromPreorder(preorder, start + 1, right - 1);
        root.right = bstFromPreorder(preorder, right, end);
        return root;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.7MB，在所有java提交中击败了11.11%的用户。

## 官方解题

&emsp;