[toc]

Given the `root` of a binary tree, consider all root to leaf paths: paths from the root to any leaf.  (A leaf is a node with no children.)

A `node` is insufficient if **every** such root to leaf path intersecting this `node` has sum strictly less than `limit`.

Delete all insufficient nodes simultaneously, and return the root of the resulting binary tree.



**Note**:

* The given tree will have between $1$ and $5000$ nodes.
* $-10^5 \le \text{node.val} \le 10^5$
* $-10^9 \le \text{limit} \le 10^9$



## 题目解读

&emsp;删除路径和小于给定值的节点，返回删除后的树。

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
    public TreeNode sufficientSubset(TreeNode root, int limit) {

    }
}
```

## 程序设计

* 采用后序遍历计算路径和，然后根据路径和返回是否要删除的标识。

```java
class Solution {
    public TreeNode sufficientSubset(TreeNode root, int limit) {
        if (root == null) return null;
        boolean flag = deleteNode(root, limit);
        // 根节点是否要删除
        return flag ? root : null;
    }

    private boolean deleteNode(TreeNode root, int limit) {
        limit -= root.val;
        // 叶节点
        if (root.left == null && root.right == null) {
            // 返回路径和是否大于等于limit
            return limit <= 0;
        }

        boolean left = false, right = false;
        if (root.left != null) left = deleteNode(root.left, limit);
        if (root.right != null) right = deleteNode(root.right, limit);
        // 删除不符合要求的分支
        if (!left) root.left = null;
        if (!right) root.right = null;
        return left || right;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：1ms，在所有java提交中击败了98.49%的用户。

内存消耗：39MB，在所有java提交中击败了38.10%的用户。

## 官方解题

&emsp;暂无，密切关注。社区精简代码如下：

```java
class Solution {
    public TreeNode sufficientSubset(TreeNode root, int limit) {
        if (root == null) return null;
        // 叶节点
        if (root.left == null && root.right == null) return root.val >= limit ? root : null;
        root.left = sufficientSubset(root.left, limit - root.val);
        root.right = sufficientSubset(root.right, limit - root.val);
        return root.left == null && root.right == null ? null : root;
    }
}
```