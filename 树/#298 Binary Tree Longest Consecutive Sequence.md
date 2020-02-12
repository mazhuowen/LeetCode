[toc]

Given a binary tree, find the length of the longest consecutive sequence path.

The path refers to any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The longest consecutive path need to be from parent to child (cannot be the reverse).



## 题目解读

&emsp;给定二叉树，输出二叉树的最大连续序列。

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
    public int longestConsecutive(TreeNode root) {
        
    }
}
```

## 程序设计

* 分析可知必须自顶向下，可以采用递归或前序遍历实现。递归思路如下：

```java
class Solution {
    private int maxLen = 0;

    public int longestConsecutive(TreeNode root) {
        longestConsecutive(root, 1);
        return maxLen;
    }

    private void longestConsecutive(TreeNode root, int len) {
        if(root == null) {
            return;
        }
        // 连续
        if(root.left != null && root.left.val == root.val + 1) {
            longestConsecutive(root.left, len + 1);
        } 
        // 不连续或无左子节点（可能是叶结点），更新maxLen
        else {
            maxLen = Math.max(maxLen, len);
            longestConsecutive(root.left, 1);
        }
        // 连续
        if(root.right != null && root.right.val == root.val + 1) {
            longestConsecutive(root.right, len + 1);
        } 
        // 不连续或无右子节点（可能是叶结点），更新maxLen
        else {
            maxLen = Math.max(maxLen, len);
            longestConsecutive(root.right, 1);
        }
    }
}
```

* 前序遍历思路如下：

```java
class Solution {
    public int longestConsecutive(TreeNode root) {
        if(root == null) {
            return 0;
        }
        int maxNum = 0;
        Stack<TreeNode> stack = new Stack<>();
        Stack<Integer> counter = new Stack<>();
        stack.push(root);
        counter.push(1);
        while(!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            int count = counter.pop();
            // 叶节点返回计数
            if(cur.left == null && cur.right == null) {
                maxNum = Math.max(maxNum, count);
            }
            if(cur.left != null) {
                stack.push(cur.left);
                // 连续
                if(cur.left.val == cur.val + 1) {
                    counter.push(count + 1);
                }
                // 不连续，则更新连续数并重置计数
                else {
                    maxNum = Math.max(maxNum, count);
                    counter.push(1);
                }
            }
            if(cur.right != null) {
                stack.push(cur.right);
                // 连续
                if(cur.right.val == cur.val + 1) {
                    counter.push(count + 1);
                }
                // 不连续，则更新连续数并重置计数
                else {
                    maxNum = Math.max(maxNum, count);
                    counter.push(1);
                }
            } 
        }
        return maxNum;
    }
}
```

## 性能分析

&emsp;递归时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：50.1MB，在所有java提交中击败了5.19%的用户。

&emsp;非递归时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：20ms，在所有java提交中击败了5.07%的用户。

内存消耗：49.6MB，在所有java提交中击败了5.19%的用户。

## 官方解题

&emsp;官方提供的思路都是递归形式。