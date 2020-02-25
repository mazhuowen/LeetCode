[toc]

Given a binary tree, find the largest subtree which is a Binary Search Tree (BST), where largest means subtree with largest number of nodes in it.

Note:
A subtree must include all of its descendants.

**Follow up:**
Can you figure out ways to solve it with O(n) time complexity?



## 题目解读

&emsp;给定二叉树，判定子树里最大的二叉查找树的结点数。

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
    public int largestBSTSubtree(TreeNode root) {

    }
}
```

## 程序设计

* 根据题目意思，二叉查找树是包含以后所有的子节点，我们可以自底向上判断。根据二叉查找树的定义，父节点需要知道子树的头结点、区间，根据题目需求，还要知道子树的节点数等信息。
* 综上，可用数组来表示这些信息，通过递归返回最终结果。

```java
class Solution {
    public int largestBSTSubtree(TreeNode root) {
        if(root == null) {
            return 0;
        }
        int[] res = bstSubtree(root);
        // 返回结点数
        return res[3]; 
    }

    private int[] bstSubtree(TreeNode root) {
        // 返回数组存储根节点值，区间开始值、结束值，结点数，是否连续
        if(root.left == null && root.right == null) {
            return new int[]{root.val, root.val, root.val, 1, 1};
        }
        int[] left = null;
        int[] right = null;
        if(root.left != null) {
            left = bstSubtree(root.left);
        }
        if(root.right != null) {
            right = bstSubtree(root.right);
        }
        // 左右结点都是连续的bst，且根节点满足条件，则合并
        if(left != null && right != null && left[4] == 1 && right[4] == 1 && root.val > left[0] && root.val > left[2] && root.val < right[0] && root.val < right[1]) {
            return new int[]{root.val, left[1], right[2], left[3] + right[3] + 1, 1};
        } 
        // 只存在右结点且满足条件，更新
        else if(left == null && right != null && right[4] == 1 && root.val < right[0] && root.val < right[1]) {
            return new int[]{root.val, root.val, right[2], right[3] + 1, 1};
        } 
        // 只存在左结点且满足条件
        else if(right == null && left != null && left[4] == 1 && root.val > left[0] && root.val > left[2]) {
            return new int[]{root.val, left[1], root.val, left[3] + 1, 1};
        }
        // 不满足条件，将连续标识置为0
        if(left != null) {
            left[4] = 0;
        }
        if(right != null) {
            right[4] = 0;
        }
        // 如果两个都存在返回节点数较大的
        if(left == null || right == null) {
            return left == null ? right : left;
        } else {
            return left[3] > right[3] ? left : right;
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：1ms，在所有java提交中击败了97.78%的用户。

内存消耗：39.2MB，在所有java提交中击败了5.13%的用户。

## 官方解题

&emsp;暂无，密切关注。社区方法与上面思路类似，只是创建对象而不是数组来表示信息。