[toc]

Given the `root` node of a binary search tree, return the sum of values of all nodes with value between `L` and `R` (inclusive).

The binary search tree is guaranteed to have unique values.



Note:

* The number of nodes in the tree is at most $10000$.
* The final answer is guaranteed to be less than $2^{31}$.



## 题目解读

&emsp;二叉树检索区间内的数字之和。

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
    public int rangeSumBST(TreeNode root, int L, int R) {

    }
}
```

## 程序设计

* 利用二叉查找树的性质前序遍历。

```java
class Solution {
    public int rangeSumBST(TreeNode root, int L, int R) {
        if (root == null) return 0;

        int sum = root.val >= L && root.val <= R ? root.val : 0;
        if (root.val < L) sum += rangeSumBST(root.right, L, R);
        else if (root.val > R) sum += rangeSumBST(root.left, L, R);
        else sum += rangeSumBST(root.right, L, R) + rangeSumBST(root.left, L, R);
        
        return sum;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：47.7MB，在所有java提交中击败了6.67%的用户。

## 官方解题

&emsp;同上，还提供了栈的实现方式。