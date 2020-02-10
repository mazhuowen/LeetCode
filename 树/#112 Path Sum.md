[toc]

Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

Note: A leaf is a node with no children.



## 题目解读

&emsp;给定二叉树和数，判断是否存在根节点到叶节点路径之和等于给定的数。

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
    public boolean hasPathSum(TreeNode root, int sum) {
        
    }
}
```

## 程序设计

* 前序遍历判断每个结点的和是否一致。

```java
import javafx.util.Pair;
class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        if(root == null) {
            return false;
        }
        Stack<Pair<TreeNode, Integer>> stack = new Stack<>();
        stack.push(new Pair(root, root.val));
        while(!stack.isEmpty()) {
            Pair<TreeNode, Integer> cur = stack.pop();
            TreeNode curNode = cur.getKey();
            // 叶节点判断
            if(curNode.left == null && curNode.right == null && cur.getValue() == sum) {
                return true;
            }
            if(curNode.left != null) {
                stack.push(new Pair(curNode.left, cur.getValue() + curNode.left.val));
            }
            if(curNode.right != null) {
                stack.push(new Pair(curNode.right, cur.getValue() + curNode.right.val));
            }
        }
        return false;
    }
}
```

* 递归实现如下。

```java
class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        // 根节点为null，返回
        if(root == null) {
            return false;
        }
        // 叶结点且符合情况
        if(root.left == null && root.right == null && root.val == sum) {
            return true;
        }
        // 递归
        if(hasPathSum(root.left, sum - root.val) || hasPathSum(root.right, sum - root.val)) {
            return true;
        }
        return false;
    }
}
```

## 性能分析

&emsp;非递归方法时间复杂度为$O(N)$，最好情况为完全二叉树，且左支路满足条件，此时为$O(\log_2N)$；空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了8.51%的用户。

内存消耗：36.3MB，在所有java提交中击败了96.49%的用户。

&emsp;递归方法时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.7MB，在所有java提交中击败了54.07%的用户。

## 官方解题

&emsp;思路同上。