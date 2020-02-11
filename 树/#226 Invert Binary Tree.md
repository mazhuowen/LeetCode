[toc]

Invert a binary tree.



## 题目解读

&emsp;反转二叉树。

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
    public TreeNode invertTree(TreeNode root) {
        
    }
}
```

## 程序设计

* 可使用递归实现。

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null) {
            return null;
        }
        // 反转
        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;
        // 向下递归
        invertTree(root.left);
        invertTree(root.right);
        return root;
    }
}
```

* 非递归可借助遍历边遍历边反转。

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null) {
            return null;
        }
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while(!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            // 反转
            TreeNode temp = cur.left;
            cur.left = cur.right;
            cur.right = temp;
            // 继续遍历
            if(cur.left != null) {
                stack.push(cur.left);
            }
            if(cur.right != null) {
                stack.push(cur.right);
            }
        }
        return root;
    }
}
```

## 性能分析

&emsp;递归时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。非递归时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：40.4MB，在所有java提交中击败了5.02%的用户。

## 官方解题

&emsp;思路同上。