[toc]

Given a binary tree `root`, a ZigZag path for a binary tree is defined as follow:

* Choose **any** node in the binary tree and a direction (right or left).
* If the current direction is right then move to the right child of the current node otherwise move to the left child.
* Change the direction from right to left or right to left.
* Repeat the second and third step until you can't move in the tree.

Zigzag length is defined as the number of nodes visited - 1. (A single node has a length of 0).

Return the longest **ZigZag** path contained in that tree.



**Constraints**:

* Each tree has at most `50000` nodes..
* Each node's value is between `[1, 100]`.



## 题目解读

&emsp;返回二叉树中最长的交替路径。

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
    public int longestZigZag(TreeNode root) {
        
    }
}
```

## 程序设计

* 最基本的思路是前序遍历，然后对每个节点向下遍历交替路径长度，这样时间复杂度为$O(N^2)$。更一般的，当前节点的交替路径取决于子结点反方向的交替路径，可以采用后序遍历的形式，返回子结点的左右交替路径，然后根据子结点得出当前的交替路径。

```java
class Solution {
    int maxLen;

    public int longestZigZag(TreeNode root) {
        zigZagLength(root);
        return maxLen;
    }

    private int[] zigZagLength(TreeNode root) {
        if (root == null) return new int[]{0, 0};

        int[] left = zigZagLength(root.left);
        int[] right = zigZagLength(root.right);

        // 当前左交替路径取决于左子结点右交替路径的长度
        int curLeft = (root.left == null ? 0 : 1) + left[1];
        // 当前右交替路径取决于右子结点左交替路径的长度
        int curRight = (root.right == null ? 0 : 1) + right[0];
        
        // 更新
        maxLen = Math.max(maxLen, Math.max(curLeft, curRight));
        return new int[]{curLeft, curRight};
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：9ms，在所有java提交中击败了49.87%的用户。

内存消耗：51.6MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方采用前序遍历的形式，传入当前是左还是右，不在当前路径的子结点则重新开始。

```java
class Solution {
    int maxLen;

    public int longestZigZag(TreeNode root) {
        
        zigZagLength(root.left, 1, true);
        zigZagLength(root.right, 1, false);
        return maxLen;
    }

    private void zigZagLength(TreeNode root, int curLen, boolean isLeft) {
        if (root == null) return;

        maxLen = Math.max(maxLen, curLen);

        if (isLeft) {
            zigZagLength(root.right, curLen + 1, !isLeft);
            // 重点，不在当前路径的子结点重新开始
            zigZagLength(root.left, 1, isLeft);
        } else {
            zigZagLength(root.left, curLen + 1, !isLeft);
            zigZagLength(root.right, 1, isLeft);
        }
    }
}
```

&emsp;时间复杂度为$O(N)$，由于是尾递归，空间复杂度为$O(1)$。

执行用时：6ms，在所有java提交中击败了97.95%的用户。

内存消耗：51.6MB，在所有java提交中击败了100.00%的用户。