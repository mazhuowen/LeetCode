[toc]

Given the root node of a binary search tree (BST) and a value. You need to find the node in the BST that the node's value equals the given value. Return the subtree rooted with that node. If such node doesn't exist, you should return NULL.

Note that an empty tree is represented by `NULL`, therefore you would see the expected output (serialized tree format) as `[]`, not `null`.



## 题目解读

&emsp;二叉查找树的搜索。

```java
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {

    }
}
```

## 程序设计

* 搜索递归形式如下

```java
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        if(root == null) {
            return null;
        }
        if(root.val == val) {
            return root;
        }
        if(root.val > val) {
            return searchBST(root.left, val);
        } else {
            return searchBST(root.right, val);
        }
    }
}
```

* 搜索迭代形式如下：

```java
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        while(root != null) {
            if(root.val == val) {
                return root;
            }
            if(root.val > val) {
                root = root.left;
            } else {
                root = root.right;
            }
        }
        return root;
    }
}
```

## 性能分析

&emsp;递归方式时间复杂度为$O(\log_2N)$，最坏$O(N)$；空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：41.8MB，在所有java提交中击败了5.01%的用户。

&emsp;迭代方式时间复杂度为$O(\log_2N)$，最坏$O(N)$；空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：42.1MB，在所有java提交中击败了5.01%的用户。

## 官方解题

&emsp;同上。