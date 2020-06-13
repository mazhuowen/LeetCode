[toc]

Given two binary trees and imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not.

You need to merge them into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of new tree.



**Note:** The merging process must start from the root nodes of both trees.



## 题目解读

&emsp;合并二叉树。

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
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {

    }
}
```

## 程序设计

* 前序遍历的形式递归。

```java
class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if (t1 == null || t2 == null) return t1 == null ? t2 : t1;
        
        t1.val += t2.val;
        t1.left = mergeTrees(t1.left, t2.left);
        t1.right = mergeTrees(t1.right, t2.right);
        return t1;
    }
}
```

* 非递归形式：

```java
class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if (t1 == null || t2 == null) return t1 == null ? t2 : t1;

        Stack<Pair> stack = new Stack<>();
        stack.push(new Pair(t1, t2));
        while (!stack.isEmpty()) {
            Pair cur = stack.pop();
            TreeNode tree1 = cur.tree1, tree2 = cur.tree2;

            // 合并到树1中
            tree1.val += tree2.val;
            // 迭代
            if (tree1.right == null) tree1.right = tree2.right;
            else if (tree2.right != null) stack.push(new Pair(tree1.right, tree2.right));

            if (tree1.left == null) tree1.left = tree2.left;
            else if (tree2.left != null) stack.push(new Pair(tree1.left, tree2.left));
        }

        return t1;
    }
}

class Pair {
    TreeNode tree1;
    TreeNode tree2;

    public Pair(TreeNode tree1, TreeNode tree2) {
        this.tree1 = tree1;
        this.tree2 = tree2;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.6MB，在所有java提交中击败了94.12%的用户。

&emsp;非递归形式时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了7.91%的用户。

内存消耗：39.9MB，在所有java提交中击败了85.29%的用户。

## 官方解题

&emsp;同上。