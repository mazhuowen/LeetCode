[toc]

Given a binary tree, return the *postorder* traversal of its nodes' values.

**Follow up:** Recursive solution is trivial, could you do it iteratively?



## 题目解读

&emsp;给定二叉树，输出后序遍历。

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
    public List<Integer> postorderTraversal(TreeNode root) {
        
    }
}
```

## 程序设计

* 递归实现如下：

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        if(root == null) {
            return res;
        }
        postorderTraversal(root, res);
        return res;
    }

    private void postorderTraversal(TreeNode root, List<Integer> res) {
        if(root == null) {
            return;
        }
        postorderTraversal(root.left, res);
        postorderTraversal(root.right, res);
        res.add(root.val);
    }
}
```

* 非递归实现需要计数入栈次数。

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        if(root == null) {
            return res;
        }
        Stack<TreeNode> stack = new Stack<>();
        Stack<Integer> counter = new Stack<>();
        stack.push(root);
        counter.push(1);
        while(!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            int count = counter.pop();
            // 遍历当前结点
            if(count++ == 3) {
                res.add(cur.val);
            } 
            // 遍历左子树
            else if(count == 2) {
                stack.push(cur);
                counter.push(count);
                if(cur.left != null) {
                    stack.push(cur.left);
                    counter.push(1);
                }
            }
            // 遍历右子树
            else {
                stack.push(cur);
                counter.push(count);
                if(cur.right != null) {
                    stack.push(cur.right);
                    counter.push(1);
                }
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;递归方法时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：41.8MB，在所有java提交中击败了5.02%的用户。

&emsp;非递归时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了64.52%的用户。

内存消耗：41.4MB，在所有java提交中击败了5.02%的用户。

## 官方解题

&emsp;思路同上，计数采用队列实现。