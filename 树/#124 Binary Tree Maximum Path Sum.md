[toc]

Given a non-empty binary tree, find the maximum path sum.

For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain at least one node and does not need to go through the root.



## 题目解读

&emsp;给定二叉树，求最大的路径值。本题中路径被定义为一条从树中任意节点出发，达到任意节点的序列。该路径**至少包含一个**节点，且不一定经过根节点。

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
    public int maxPathSum(TreeNode root) {
        
    }
}
```

## 程序设计

* 首先题目中路径被定义为一条从树中任意节点出发，达到任意节点的序列。一个结点的最大路径则可分析得出：如果结点有两个子节点，则对应两个到叶结点的链路，选择两个最大的链路与当前结点相加就得到了当前结点的最大路径之和，如果有一个结点就只有一条链路，没有结点则最大路径和就是当前结点值；这样自底向上，每遍历一个结点就计算其最大路径之和，同时返回其最大链路。
* 因为是自底向上，可通过递归实现，递归函数返回当前结点的最大链路，不是最大路径之和，故还需要额外的变量记录最大路径和。

```java
class Solution {
    // 记录
    private int maxSum = Integer.MIN_VALUE;

    public int maxPathSum(TreeNode root) {
        pathSum(root);
        return maxSum;
    }

    // 返回当前结点的最大路径
    private int pathSum(TreeNode root) {
        if(root == null) {
            return 0;
        }
        // 左子树最大路径，小于0则丢弃
        int left = Math.max(pathSum(root.left), 0);
        // 右子树同理
        int right = Math.max(pathSum(root.right), 0);
        // 当前结点最大路径为左右子树最大路径加结点本身
        int curSum = root.val + left + right;
        // 更新最大路径
        maxSum = Math.max(maxSum, curSum);
        // 返回当前结点所在链路的最大路径（非常重要，和curSum意义不一样）
        return root.val + Math.max(left, right);
    }
}
```

## 性能分析

&emsp;递归时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：1ms，在所有java提交中击败了99.95%的用户。

内存消耗：49.2MB，在所有java提交中击败了5.02%的用户。

## 官方解题

&emsp;思路同上，通过递归解决。