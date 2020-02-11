[toc]

Given a binary tree, return the *preorder* traversal of its nodes' values.

**Follow up:** Recursive solution is trivial, could you do it iteratively?



## 题目解读

&emsp;给定二叉树，得到前序遍历。

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
    public List<Integer> preorderTraversal(TreeNode root) {
        
    }
}
```

## 程序设计

* 递归实现如下：

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        if(root == null) {
            return res;
        }
        preorderTraversal(root, res);
        return res;
    }

    // 递归方法
    private void preorderTraversal(TreeNode root, List<Integer> res) {
        if(root == null) {
            return;
        }
        res.add(root.val);
        preorderTraversal(root.left, res);
        preorderTraversal(root.right, res);
    }
}
```

* 非递归实现如下：

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        if(root == null) {
            return res;
        }
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while(!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            res.add(cur.val);
            if(cur.right != null) {
                stack.push(cur.right);
            }
            if(cur.left != null) {
                stack.push(cur.left);
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;递归方法时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：41.2MB，在所有java提交中击败了5.04%的用户。

&emsp;非递归方法时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了62.14%的用户。

内存消耗：41.2MB，在所有java提交中击败了5.04%的用户。

## 官方解题

&emsp;思路同上。

