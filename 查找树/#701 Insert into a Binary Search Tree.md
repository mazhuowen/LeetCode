[toc]

Given the root node of a binary search tree (BST) and a value to be inserted into the tree, insert the value into the BST. Return the root node of the BST after the insertion. It is guaranteed that the new value does not exist in the original BST.

Note that there may exist multiple valid ways for the insertion, as long as the tree remains a BST after insertion. You can return any of them.



## 题目解读

&emsp;二叉查找树插入一个值。

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
    public TreeNode insertIntoBST(TreeNode root, int val) {
        
    }
}
```

## 程序设计

* 递归形式：

```java
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if(root == null) {
            return new TreeNode(val);
        }
        // 已经存在
        if(root.val == val) {
            return root;
        }
        // 递归
        if(root.val > val) {
            root.left = insertIntoBST(root.left, val);
        } else {
            root.right = insertIntoBST(root.right, val);
        }
        return root;
    }
}
```

* 迭代形式：

```java
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if(root == null) {
            return new TreeNode(val);
        }
        TreeNode cur = root;
        while(cur != null) {
            if(cur.val == val) {
                break;
            }
            if(cur.val < val) {
                if(cur.right == null) {
                    cur.right = new TreeNode(val);
                    break;
                }
                cur = cur.right;
            } else {
                if(cur.left == null) {
                    cur.left = new TreeNode(val);
                    break;
                }
                cur = cur.left;
            }
        }
        return root;
    }
}
```

## 性能分析

&emsp;递归时间复杂度为$O(\log_2N)$，最坏$O(N)$；空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：42.2MB，在所有java提交中击败了5.11%的用户。

&emsp;迭代时间复杂度为$O(\log_2N)$，最坏$O(N)$；空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：42.2MB, 在所有java提交中击败了5.11%的用户。

## 官方解题

&emsp;同上。