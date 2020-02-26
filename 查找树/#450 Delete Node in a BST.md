[toc]

Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node reference (possibly updated) of the BST.

Basically, the deletion can be divided into two stages:

* Search for a node to remove.
* If the node is found, delete the node.

Note: Time complexity should be $O(\text{height of tree})$.



## 题目解读

&emsp;实现二叉查找树的删除功能，返回根节点。

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
    public TreeNode deleteNode(TreeNode root, int key) {

    }
}
```

## 程序设计

* 二叉树删除首先是查找，未找到直接返回；查找到后，如果是叶节点直接删除，如果只有一个子节点，子节点代替并删除；如果有两个子节点，则查找代替子节点，删除。
* 非递归实现如下：

```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        TreeNode parent = null, cur = root;
        while(cur != null) {
            if(cur.val == key) {
                break;
            }
            parent = cur;
            if(cur.val > key) {
                cur = cur.left;
            } else {
                cur = cur.right;
            }
        }
        // 未查找到，直接返回
        if(cur == null) return root;

        // 待删除结点有两个子节点，查找替换结点
        if(cur.left != null && cur.right != null) {
            parent = cur;
            TreeNode replace = cur.right;
            while(replace.left != null) {
                parent = replace;
                replace = replace.left;
            }
            // 替换值
            cur.val = replace.val;
            // 待删除结点变更为替换结点
            cur = replace;
        }

        // 叶结点
        if(cur.left == null && cur.right == null) {
            // 待删除结点是根结点
            if(parent == null) {
                root = null;
            } 
            // 叶节点直接置空
            else if(parent.left == cur) {
                parent.left = null;
            } else {
                parent.right = null;
            }
        }
        // 单子节点的结点
        else {
            // 待删除结点是根结点
            if(parent == null) {
                root = cur.left == null ? cur.right : cur.left;
            } 
            // 父节点直接指向待删除结点的子节点
            else if(parent.left == cur) {
                parent.left = cur.left == null ? cur.right : cur.left;
            } else {
                parent.right = cur.left == null ? cur.right : cur.left;
            }
        }
        return root;
    }
}
```

* 递归实现如下：

```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if(root == null) {
            return root;
        }
        // 递归删除
        if(root.val > key) {
            root.left = deleteNode(root.left, key);
        } else if(root.val < key) {
            root.right = deleteNode(root.right, key);
        } else {
            // 当前结点是叶节点，删除
            if(root.left == null && root.right == null) {
                root = null;
            } 
            // 有两个子节点
            else if(root.left != null && root.right != null) {
                // 找到替换结点
                TreeNode replace = root.right;
                while(replace.left != null) {
                    replace = replace.left;
                }
                root.val = replace.val;
                // 递归删除
                root.right = deleteNode(root.right, replace.val);
            }
            // 有一个子节点
            else {
                root = root.left == null ? root.right : root.left;
            }
        }
        return root;
    }
}
```

## 性能分析

&emsp;非递归实现时间复杂度为$O(\log_2N)$，最坏$O(N)$；空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：42.2MB, 在所有java提交中击败了5.20%的用户。

&emsp;递归实现时间复杂度为$O(\log_2N)$，最坏$O(N)$；空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：41.9MB，在所有java提交中击败了5.20%的用户。

## 官方解题

&emsp;同上。