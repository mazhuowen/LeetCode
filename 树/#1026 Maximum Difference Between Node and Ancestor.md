[toc]

Given the `root` of a binary tree, find the maximum value `V` for which there exists **different** nodes `A` and `B` where `V = |A.val - B.val|` and `A` is an ancestor of `B`.

(`A` node `A` is an ancestor of `B` if either: any child of `A` is equal to `B`, or any child of `A` is an ancestor of `B`.)



**Note**:

* The number of nodes in the tree is between $2$ and $5000$.
* Each node will have value between $0$ and $100000$.



## 题目解读

&emsp;给定二叉树，找到最大的祖先节点与子孙节点的差值。

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
    public int maxAncestorDiff(TreeNode root) {
        
    }
}
```

## 程序设计

* 

```java
class Solution {
    // 最大差值
    int maxDiff = 0;

    public int maxAncestorDiff(TreeNode root) {
        minOrMaxChild(root);
        return maxDiff;
    }

    // 返回子树中节点中最小值和最大值
    private int[] minOrMaxChild(TreeNode root) {
        if (root == null) return new int[]{Integer.MAX_VALUE, Integer.MIN_VALUE};

        int[] left = minOrMaxChild(root.left);
        int[] right = minOrMaxChild(root.right);

        int min = Math.min(left[0], right[0]);
        int max = Math.max(left[1], right[1]);

        // 更新差值
        if (min != Integer.MAX_VALUE && root.val - min > maxDiff) maxDiff =  root.val - min;
        if (max != Integer.MIN_VALUE && max - root.val > maxDiff) maxDiff = max - root.val;
        return new int[]{Math.min(min, root.val), Math.max(max, root.val)};
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：40.5MB，在所有java提交中击败了33.48%的用户。

## 官方解题

&emsp;暂无，密切关注。