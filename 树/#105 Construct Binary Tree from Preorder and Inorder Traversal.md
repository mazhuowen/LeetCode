[toc]

Given preorder and inorder traversal of a tree, construct the binary tree.



**Note**:
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

&emsp;官方思路同上，加入记录中序遍历节点值与位置的哈希表来优化递归中的循环查找；除了上述思路还提供了迭代法。由于题目说明每个节点值都是唯一的，可利用前序遍历的特性，即相邻两个节点$p$、$s$，要么$s$是$p$的左子节点，要么是$p$祖先路径上某个节点的右子节点；而是左是右可通过中序遍历判断。

```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if (preorder == null || inorder == null || preorder.length == 0 || inorder.length == 0) return null;

        TreeNode root = new TreeNode(preorder[0]);
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        int idx = 0;
        for (int i = 1; i < preorder.length; i++) {
            TreeNode cur = new TreeNode(preorder[i]), pre = stack.peek();
            // 栈顶值与中序遍历值不同，表明左分支还未遍历完
            if (pre.val != inorder[idx]) {
                pre.left = cur;
                stack.push(cur);
            } 
            // 栈顶值与中序遍历值相等，表明栈顶节点的左子树已维护完成
            else {
                // 将维护完的节点出栈
                while (!stack.isEmpty() && stack.peek().val == inorder[idx]) {
                    pre = stack.pop();
                    idx++;
                }
                pre.right = cur;
                stack.push(cur);
            }
        }
        return root;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了76.49%的用户。

内存消耗：39.6MB，在所有java提交中击败了99.02%的用户。

&emsp;社区巧妙的采用递归实现上述思路：

```java
class Solution {
    // 前序遍历、中序遍历索引
    int pre = 0, in = 0;

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if (preorder == null || inorder == null || preorder.length == 0 || inorder.length == 0) return null;

        return buildTree(preorder, inorder, Integer.MAX_VALUE);
    }

    // stop为中序数组遍历的结束点
    private TreeNode buildTree(int[] preorder, int[] inorder, int stop) {
        if (pre >= preorder.length) return null;
        if (inorder[in] == stop) {
            in++;
            return null;
        }

        TreeNode root = new TreeNode(preorder[pre++]);
        // 左子树的结束点为当前根节点
        root.left = buildTree(preorder, inorder, root.val);
        root.right = buildTree(preorder, inorder, stop);
        return root;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.8MB，在所有java提交中击败了90.81%的用户。