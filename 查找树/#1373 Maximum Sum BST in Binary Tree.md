[toc]

Given **a binary tree** root, the task is to return the maximum sum of all keys of **any** sub-tree which is also a Binary Search Tree (BST).

Assume a BST is defined as follows:

* The left subtree of a node contains only nodes with keys **less than** the node's key.
* The right subtree of a node contains only nodes with keys **greater than** the node's key.
* Both the left and right subtrees must also be binary search trees.



Constraints:

* The given binary tree will have between `1` and `40000` nodes.
* Each node's value is between $[-4 * 10^4 , 4 * 10^4]$.



## 题目解读

&emsp;在二叉树中查找元素和最大的二叉搜索树。

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
    public int maxSumBST(TreeNode root) {
        
    }
}
```

## 程序设计

* 二叉搜索子树当前结点到达叶节点都满足大小条件，可以自底向上判断，如果两个子树是二叉搜索树，而当前结点与两个根满足大小关系，则当前结点为根的子树也是二叉搜索树；反之不是。

```java
class Solution {
    int maxSum;

    public int maxSumBST(TreeNode root) {
        sumBST(root);
        return maxSum;
    }

    private int[] sumBST(TreeNode root) {
        // 空节点是二叉搜索树，且和为0
        if (root == null) return new int[]{1, 0};

        int[] left = sumBST(root.left);
        int[] right = sumBST(root.right);

        int sum = root.val;
        // 左右形成搜索树
        if (left[0] == 1 && (root.left == null || root.left.val < root.val)
                && right[0] == 1 && (root.right == null || root.right.val > root.val)) {
            sum += left[1] + right[1];
            // 更新关系
            maxSum = Math.max(maxSum, sum);
            return new int[]{1, sum};
        } else {
            // 非二叉搜索树，和为0
            return new int[]{0, 0};
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：7ms，在所有java提交中击败了84.84%的用户。

内存消耗：52.4MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。