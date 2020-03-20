[toc]

Given the `root` of a binary tree, find the maximum average value of any subtree of that tree.

(A subtree of a tree is any node of that tree plus all its descendants. The average value of a tree is the sum of its values, divided by the number of nodes.)

 

**Note:**

* The number of nodes in the tree is between `1` and `5000`.
* Each node will have a value between `0` and `100000`.
* Answers will be accepted as correct if they are within `10^-5` of the correct answer.



## 题目解读

&emsp;给定二叉树，返回所有子树中最大的平均值。

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
    public double maximumAverageSubtree(TreeNode root) {

    }
}
```

## 程序设计

* 后序遍历计算当前结点所在根的子树结点数目和和，然后更新最大平均值。

```java
class Solution {
    private double max = 0D;

    public double maximumAverageSubtree(TreeNode root) {
        subTreeSum(root);
        return max;
    }
	// 返回当前树的结点总和和数目
    private int[] subTreeSum(TreeNode root) {
        if (root == null) return new int[]{0, 0};
        int curSum = root.val, curNum = 1;
        int[] left = subTreeSum(root.left);
        int[] right = subTreeSum(root.right);
        curSum += left[0] + right[0];
        curNum += left[1] + right[1];
        // 更新均值
        max = Math.max((double)curSum / curNum, max);
        return new int[]{curSum, curNum};
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：41.6MB，在所有java提交中击败了12.50%的用户。

## 官方解题

&emsp;暂无，密切关注。