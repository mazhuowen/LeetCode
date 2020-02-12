[toc]

Given a binary tree, count the number of uni-value subtrees.

A Uni-value subtree means all nodes of the subtree have the same value.



## 题目解读

&emsp;统计给定的二叉树里，同值子树的个数，同值子树指子树的所有结点值必须相等。

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
    public int countUnivalSubtrees(TreeNode root) {
        
    }
}
```

## 程序设计

* 首先想到的是自底向上，首先叶结点肯定是同值子树，向上遍历，如果父结点与其同值子树结点的值相同，则也是一棵同值子树；如果父结点与子节点值不同或子节点的子树就不是同值子树，则必然不是同值子树。

```java
class Solution {
    private int count = 0;

    public int countUnivalSubtrees(TreeNode root) {
        univalSubtrees(root);
        return count;
    }

    // 返回是否是同值子树
    private boolean univalSubtrees(TreeNode root) {
        if(root == null) {
            return true;
        }
        // 叶结点
        if(root.left == null && root.right == null) {
            count++;
            return true;
        }
        // 需要遍历完，不能放到条件里面
        boolean left = univalSubtrees(root.left);
        boolean right = univalSubtrees(root.right);
        // 子节点有一个不是同值子树，则当前结点组成的子树肯定不是
        if(!left || !right) {
            return false;
        }
        // 判断当前结点与同值子树结点是否一致
        if(root.left != null && root.val != root.left.val) {
            return false;
        }
        if(root.right != null && root.val != root.right.val) {
            return false;
        }
        // 是同值子树，计数加一
        count++;
        return true;
    }
}
```



## 性能分析

&emsp;递归方法时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：45.3MB，在所有java提交中击败了5.05%的用户。

## 官方解题

&emsp;思路一致。