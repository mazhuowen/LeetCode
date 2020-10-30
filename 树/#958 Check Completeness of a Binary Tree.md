[toc]

Given a binary tree, determine if it is a complete binary tree.

Definition of a complete binary tree from Wikipedia:
In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and $2^h$ nodes inclusive at the last level $h$.

 

**Note:**

* The tree will have between $1$ and $100$ nodes.



## 题目解读

&emsp;判断一棵二叉树是不是满二叉树。

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
    public boolean isCompleteTree(TreeNode root) {

    }
}
```

## 程序设计

* 参考官方解题，对树结构进行编号，最后编号的最大值就是结点数目，如果不满足条件就不是满二叉树。

```java
class Solution {
    int count, maxNo;

    public boolean isCompleteTree(TreeNode root) {
        // 结点数和最大编号
        count = 0;
        maxNo = 0;
        isCompleteTree(root, 1);
        return count == maxNo;
    }

    private void isCompleteTree(TreeNode root, int no) {
        if (root == null) return;
        count++;
        maxNo = Math.max(maxNo, no);

        isCompleteTree(root.left, 2 * no);
        isCompleteTree(root.right, 2 * no + 1);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.5MB，在所有java提交中击败了25.00%的用户。

## 官方解题

&emsp;见上。