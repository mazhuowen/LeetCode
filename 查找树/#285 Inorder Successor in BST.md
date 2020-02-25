[toc]

Given a binary search tree and a node in it, find the in-order successor of that node in the BST.

The successor of a node p is the node with the smallest key greater than p.val.



Note:

* If the given node has no in-order successor in the tree, return `null`.
* It's guaranteed that the values of the tree are unique.



## 题目解读

&emsp;查找二叉树中给定结点的中序遍历后继结点。

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
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        
    }
}
```

## 程序设计

* 首先想到的是迭代方式，对于结点`p`存在右子节点，则后继在右子节点的左支路末端；不存在则后继在祖先结点路径上。

```java
class Solution {
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        if(root == null || p == null) {
            return null;
        }
         // 如果存在右子树，查找后继
        if(p.right != null) {
            p = p.right;
            while(p.left != null) {
                p = p.left;
            }
            return p;
        }
        // 不存在右子树，则记录路径，遍历祖先结点
        Stack<TreeNode> path = new Stack<>();
        // 查找p
        while(root != null) {
            path.push(root);
            if(root.val == p.val) {
                break;
            }
            if(root.val > p.val) {
                root = root.left;
            } else {
                root = root.right;
            }
        }
        // 后继结点为祖先结点是其父节点的左子节点的结点
        TreeNode cur = path.pop();
        while(!path.isEmpty() && path.peek().right == cur) {
            cur = path.pop();
        }
        // 不存在
        if(path.isEmpty()) {
            return null;
        }
        return path.peek();
    }
}
```

* 递归形式如下：

```java
class Solution {
    private TreeNode pre;
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        if(root == null || p == null) {
            return null;
        }
        TreeNode left = inorderSuccessor(root.left, p);
        if(left != null) return left;
        // 比较前驱
        if(pre != null && p == pre) {
            return root;
        }
        // 记录前驱
        pre = root;
        return inorderSuccessor(root.right, p);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_2N)$，最坏$O(N)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了99.64%的用户。

内存消耗：41.9MB，在所有java提交中击败了5.77%的用户。

&emsp;递归时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：3ms，在所有java提交中击败了99.64%的用户。

内存消耗：42.2MB，在所有java提交中击败了5.77%的用户。

## 官方解题

&emsp;暂无，密切关注。社区有巧妙利用二叉查找树性质，进行一次查找得到后继的思路。结点`p`的后继有以下情况：

* 存在右子树，查找左支路末端；
* 不存在右子树，查找其父节点的左子节点是其本身的祖先结点；

将这个思路应用到搜索，首先是查找`p`，即当前结点小于`p`时去右子树，否则去左子树查找；上面的情况需要记录结点，因为可能`p`没有右子节点，则后继结点在祖先路上。在查到`p`后，去右子树查找左支路，如果存在，则记录结点。

```java
class Solution {
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        if(root == null || p == null) {
            return null;
        }
        TreeNode res = null;
        while(root != null) {
            // 在右子树查找大于p的值
            if(root.val <= p.val) {
                root = root.right;
            } 
            // 查找左支路
            else {
                // 更新结点
                if(res == null || res.val > root.val) {
                    res = root;
                }
                root = root.left;
            }
        }
        return res;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：3ms，在所有java提交中击败了99.64%的用户。

内存消耗：42.2MB，在所有java提交中击败了5.77%的用户。