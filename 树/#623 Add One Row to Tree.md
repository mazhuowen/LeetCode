[toc]

Given the root of a binary tree, then value `v` and depth `d`, you need to add a row of nodes with value `v` at the given depth `d`. The root node is at depth 1.

The adding rule is: given a positive integer depth `d`, for each NOT null tree nodes `N` in depth `d - 1`, create two tree nodes with value `v` as `N`'s left subtree root and right subtree root. And `N`'s **original left subtree** should be the left subtree of the new left subtree root, its **original right subtree** should be the right subtree of the new right subtree root. If depth d is 1 that means there is no depth `d - 1` at all, then create a tree node with value **v** as the new root of the whole original tree, and the original tree is the new root's left subtree.

Note:

* The given d is in range `[1, maximum depth of the given tree + 1]`.
* The given binary tree has at least one tree node.



## 题目解读

&emsp;题目要求在`d - 1`层所有结点及其子节点之间插入结点。

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
    public TreeNode addOneRow(TreeNode root, int v, int d) {

    }
}
```

## 程序设计

* 层次遍历。

```java
class Solution {
    public TreeNode addOneRow(TreeNode root, int v, int d) {
        if (root == null || d < 1) return root;

        // 特殊情况处理
        if (d == 1) {
            TreeNode newRoot = new TreeNode(v);
            newRoot.left = root;
            return newRoot;
        }

        // 寻找第d - 1层
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        int floor = 1;
        while (floor < d - 1 && !queue.isEmpty()) {
            floor++;
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode cur = queue.poll();

                if (cur.left != null) queue.add(cur.left);
                if (cur.right != null) queue.add(cur.right);
            }
        }
        if (floor != d -1) throw new IllegalArgumentException("invalid param");

        // 层次遍历
        while (!queue.isEmpty()) {
            TreeNode cur = queue.poll();
            TreeNode newLeft = new TreeNode(v);
            TreeNode newRight = new TreeNode(v);

            // 插入新结点
            newLeft.left = cur.left;
            newRight.right = cur.right;
            cur.left = newLeft;
            cur.right = newRight;
        }

        return root;
    }
}
```

* 前序遍历形式如下：

```java
class Solution {
    public TreeNode addOneRow(TreeNode root, int v, int d) {
        if (root == null || d < 1) return root;

        if (d == 1) {
            TreeNode newRoot = new TreeNode(v);
            newRoot.left = root;
            return newRoot;
        }

        preOrder(root, 1, v, d);
        return root;
    }

    private void preOrder(TreeNode root, int floor, int v, int d) {
        if (root == null) return;

        // 找到d - 1层的一个节点
        if (floor == d - 1) {
            TreeNode left = new TreeNode(v);
            TreeNode right = new TreeNode(v);
            left.left = root.left;
            right.right = root.right;
            root.left = left;
            root.right = right;
        } else {
            preOrder(root.left, floor + 1, v, d);
            preOrder(root.right, floor + 1, v, d);
        }
    }
}
```

## 性能分析

&emsp;层次遍历时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了83.05%的用户。

内存消耗：39.7MB，在所有java提交中击败了7.32%的用户。

&emsp;前序遍历时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.8MB，在所有java提交中击败了7.32%的用户。

## 官方解题

&emsp;同上。