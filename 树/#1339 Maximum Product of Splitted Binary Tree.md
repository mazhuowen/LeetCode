[toc]

Given a binary tree `root`. Split the binary tree into two subtrees by removing $1$ edge such that the product of the sums of the subtrees are maximized.

Since the answer may be too large, return it modulo $10^9 + 7$.



**Constraints**:

* Each tree has at most $50000$ nodes and at least $2$ nodes.
* Each node's value is between $[1, 10000]$.



## 题目解读

&emsp;将二叉树分为两部分，求最大的乘积。

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
    public int maxProduct(TreeNode root) {
        
    }
}
```

## 程序设计

* 首先计算二叉树的节点值之和，然后后序遍历划分边，比较更新最大乘积。

```java
class Solution {
    private final static int MOD = 1_000_000_007;
    long max = Integer.MIN_VALUE;

    public int maxProduct(TreeNode root) {
        if (root == null) throw new IllegalArgumentException("invalid param");

        // 树的节点值之和
        int sum = getSum(root);
        // 尝试切分，选择最大值
        splitTree(root, sum);
        return (int)(max % MOD);
    }

    private int getSum(TreeNode root) {
        if (root == null) return 0;
        return root.val + getSum(root.left) + getSum(root.right);
    }

    private int splitTree(TreeNode root, int sum) {
        if (root == null) return 0;

        // 断开左子树或右子树，选择乘积和最大的结果
        int left = splitTree(root.left, sum);
        int right = splitTree(root.right, sum);

        max = Math.max(max, Math.max((long)left * (sum - left), (long)right * (sum - right)));
        // 返回子树之和
        return root.val + left + right;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：11ms，在所有java提交中击败了59.82%的用户。

内存消耗：55.5MB，在所有java提交中击败了34.06%的用户。

## 官方解题

&emsp;官方思路类似。