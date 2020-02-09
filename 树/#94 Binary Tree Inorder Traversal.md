[toc]

Given a binary tree, return the *inorder* traversal of its nodes' values.

**Follow up:** Recursive solution is trivial, could you do it iteratively?



## 题目解读

&emsp;给定二叉树，输出中序遍历。

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
    public List<Integer> inorderTraversal(TreeNode root) {
        
    }
}
```

## 程序设计

* 首先想到的是使用递归实现中序遍历。

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        inorder(root, res);
        return res;
    }

    private void inorder(TreeNode root, List<Integer> res) {
        if(root == null) {
            return;
        }
        // 左子树
        if(root.left != null) {
            inorder(root.left, res);
        }
        // 根
        res.add(root.val);
        // 右子树
        if(root.right != null) {
            inorder(root.right, res);
        }
    }
}
```

* 非递归实现需要计数结点入栈次数，在结点入栈两次的时候遍历当前结点，并入栈右子树，否则入栈左子树。

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        if(root == null) {
            return res;
        }
        // 记录遍历结点
        Stack<TreeNode> stack = new Stack<>();
        // 记录入栈次数
        Stack<Integer> counter = new Stack<>();
        stack.push(root);
        counter.push(1);
        while(!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            int count = counter.pop();
            // 入栈2次，遍历根节点，并入栈右子树
            if(count++ == 2) {
                res.add(cur.val);
                if(cur.right != null) {
                    stack.push(cur.right);
                    counter.push(1);
                }
            } 
            // 入栈1次，则将左子树入栈
            else {
                stack.push(cur);
                counter.push(count);
                if(cur.left != null) {
                    stack.push(cur.left);
                    counter.push(1);
                }
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;递归调用需要遍历$N$个点，时间复杂度为$O(N)$，空间复杂度为最坏情况下二叉树是一个链表，此时需要$Q(N)$，平均情况由于递归$\log_2N$轮，每轮需要常量级的空间，总共需要$O(\log_2N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：35.1MB，在所有java提交中击败了5.16%的用户。

&emsp;非递归调用时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了69.88%的用户。

内存消耗：34.5MB，在所有java提交中击败了84.35%的用户。

## 官方解题

&emsp;官方非递归实现采用另一种形式。栈只保存结点的左支路的结点

```java
public class Solution {
    public List < Integer > inorderTraversal(TreeNode root) {
        List < Integer > res = new ArrayList < > ();
        Stack < TreeNode > stack = new Stack < > ();
        TreeNode curr = root;
        while (curr != null || !stack.isEmpty()) {
            // 将当前结点的左支路入栈
            while (curr != null) {
                stack.push(curr);
                curr = curr.left;
            }
            // 弹出栈
            curr = stack.pop();
            res.add(curr.val);
            // 将当前结点设为右子树
            curr = curr.right;
        }
        return res;
    }
}
```