[toc]

Given the `root` of a binary tree with `N` nodes, each node in the tree has `node.val` coins, and there are `N` coins total.

In one move, we may choose two adjacent nodes and move one coin from one node to another.  (The move may be from parent to child, or from child to parent.)

Return the number of moves required to make every node have exactly one coin.



**Note:**

* $1\le N \le 100$
* $0 \le \text{node.val} \le N$



## 题目解读

&emsp;将二叉树结点中的硬币均匀移动到哥各个顶点，返回移动次数。

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
    public int distributeCoins(TreeNode root) {

    }
}
```

## 程序设计

* 首先给定二叉树结构的情况下，任意一种移动方法的移动次数相等。自底向上，将多余（或缺失）的硬币、当前的移动数返回上层结点。

```java
class Solution {
    public int distributeCoins(TreeNode root) {
        int[] res = moveCoins(root);
        return res[1];
    }

    // 返回数组表示多余的硬币即移动到当前根节点所需的步数
    private int[] moveCoins(TreeNode root) {
        if (root == null) return new int[]{0, 0};

        int[] left = moveCoins(root.left);
        int[] right = moveCoins(root.right);

        // 表示当前结点多余的硬币（如果为负数则表示缺少的硬币）
        int count = left[0] + right[0] + root.val - 1;
        // 将左右子节点多余（或缺少）的硬币移动到当前结点的次数加上左右子节点各自移动的次数
        int move =  Math.abs(left[0]) + Math.abs(right[0]) + left[1] + right[1];

        return new int[]{count, move};
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.6MB，在所有java提交中击败了33.33%的用户。

## 官方解题

&emsp;同上。