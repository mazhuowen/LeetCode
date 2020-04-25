[toc]

Given a binary search tree and the lowest and highest boundaries as `L` and `R`, trim the tree so that all its elements lies in `[L, R]` (R >= L). You might need to change the root of the tree, so the result should return the new root of the trimmed binary search tree.



## 题目解读

&emsp;裁剪二叉查找树。

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
    public TreeNode trimBST(TreeNode root, int L, int R) {

    }
}
```

## 程序设计

* 删除二叉查找树中超出范围的结点。

```java
class Solution {
    public TreeNode trimBST(TreeNode root, int L, int R) {
        if (root == null) return root;

        TreeNode left = trimBST(root.left, L, R);
        TreeNode right = trimBST(root.right, L, R);

        // 不在区间内，需要删除当前结点，查找右子树的左末端（或左子树的右末端）
        if (root.val < L || root.val > R) {
            // 当前结点需删除，只有一个子节点，返回该结点
            if (right == null) root = left;
            else if (left == null) root = right;
            // 查找右子树的左末端替换
            else {
                root = right;
                TreeNode pre = null;
                while (root.left != null) {
                    pre = root;
                    root = root.left;
                }
                // 删除
                if (pre != null) pre.left = null;
                // 拼接
                root.left = left;
                root.right = right;
            }
        } else {
            root.left = left;
            root.right = right;
        }

        return root;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.5MB，在所有java提交中击败了25.00%的用户。

## 官方解题

&emsp;官方提供更简洁的形式。上述算法未利用到二叉查找树的有序性，如果当前结点小于最小值，则其左子树全部裁剪；如果大于最大值，则其右子树全部裁剪。

```java
class Solution {
    public TreeNode trimBST(TreeNode root, int L, int R) {
        if (root == null) return root;
        // 当前点不符合要求
        if (root.val > R) return trimBST(root.left, L, R);
        if (root.val < L) return trimBST(root.right, L, R);
		// 当前点符合要求
        root.left = trimBST(root.left, L, R);
        root.right = trimBST(root.right, L, R);
        return root;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.7MB，在所有java提交中击败了25.00%的用户。