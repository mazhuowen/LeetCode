[toc]

Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the **longest** path between any two nodes in a tree. This path may or may not pass through the root.

Note: The length of path between two nodes is represented by the number of edges between them.



## 题目解读

&emsp;一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过根结点。

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
    public int diameterOfBinaryTree(TreeNode root) {

    }
}
```

## 程序设计

* 实质就是计算一个结点的两条路径的长度的最大值。

```java
class Solution {
    private int max = 0;
    public int diameterOfBinaryTree(TreeNode root) {
        if(root == null) {
            return 0;
        }
        countLen(root);
        // 边为结点数减一
        return max - 1;
    }

    private int countLen(TreeNode root) {
        if(root == null) {
            return 0;
        }
        int left = countLen(root.left);
        int right = countLen(root.right);
        // 左右子节点的半径
        int cur = left + right + 1;
        max = Math.max(max, cur);
        // 返回最大的单链表长度
        return Math.max(left, right) + 1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.4MB，在所有java提交中击败了5.07%的用户。

## 官方解题

&emsp;同上。