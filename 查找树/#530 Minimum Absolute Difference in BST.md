[toc]

Given a binary search tree with non-negative values, find the minimum absolute difference between values of any two nodes.



**Note:** There are at least two nodes in this BST.



## 题目解读

&emsp;计算给定非负数二叉查找树中任意两个结点值的最小绝对值差。

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
    public int getMinimumDifference(TreeNode root) {

    }
}
```

## 程序设计

* 由于是非负数，最小的差值只能存在与中序序列前后元素，问题转化为遍历问题。使用递归方法实现如下：

```java
class Solution {
    // 标记pre是否赋值
    private boolean flag = false;
    private int pre;
    private int diff = Integer.MAX_VALUE;

    public int getMinimumDifference(TreeNode root) {
        pre = root.val;
        inorder(root);
        return diff;
    }

    private void inorder(TreeNode root) {
        if(root == null) {
            return;
        }
        inorder(root.left);
        if(flag) {
            diff = Math.min(diff, root.val - pre);
        }
        pre = root.val;
        flag = true;
        inorder(root.right);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：41.4MB，在所有java提交中击败了5.06%的用户。

## 官方解题

&emsp;暂无，密切关注。