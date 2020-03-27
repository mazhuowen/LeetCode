[toc]

Two players play a turn based game on a binary tree.  We are given the `root` of this binary tree, and the number of nodes `n` in the tree.  n is odd, and each node has a distinct value from `1` to `n`.

Initially, the first player names a value `x` with $1 \le x \le n$, and the second player names a value `y` with $1 \le y \le n$ and $y \ne x$.  The first player colors the node with value $x$ red, and the second player colors the node with value $y$ blue.

Then, the players take turns starting with the first player.  In each turn, that player chooses a node of their color (red if player 1, blue if player 2) and colors an **uncolored** neighbor of the chosen node (either the left child, right child, or parent of the chosen node.)

If (and only if) a player cannot choose such a node in this way, they must pass their turn.  If both players pass their turn, the game ends, and the winner is the player that colored more nodes.

You are the second player.  If it is possible to choose such a `y` to ensure you win the game, return `true`.  If it is not possible, return `false`.



Constraints:

* `root` is the root of a binary tree with n nodes and distinct node values from `1` to `n`.
* `n` is odd.
* $1 \le x \le n \le 100$



## 题目解读

&emsp;二叉树着色，两个人分别对二叉树着色，每个人只能对自己颜色的未着色邻接结点着色，如果无可着色结点，则本轮结束，最后着色多的人获胜。题目根据第一个人首选的点，判断第二个人是否可能胜利。

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
    public boolean btreeGameWinningMove(TreeNode root, int n, int x) {

    }
}
```

## 程序设计

* 一个点最多有相邻的三个节点着色，即左右子节点、父结点。若果这三个结点中的一个结点被另一个人着色，则一条路径会被隔开，无法着色。
* 根据上述讨论，第二个人最佳首选点就是第一个人首选点的左右、父节点中的一个，这样就会隔开一条路径不会被第一个人着色。
* 判断第二个人是否能赢实际就是判断隔开的这条路径和剩下的树的部分哪个结点多。有了这个思路，首先查找到第一个人着色的结点，然后遍历统计其左右子树的尺寸，由于树的尺寸已知，故第一个人首选的点隔开的三部分尺寸已知。
* 如果这三部分，存在一部分尺寸大于总数的一半（题目提示总数是奇数），则只要第二个人选这条路径，则稳赢。

```java
class Solution {
    public boolean btreeGameWinningMove(TreeNode root, int n, int x) {
        if (root == null) return false;
		// 定位到x点
        TreeNode target = findNode(root, x);
        if (target == null) throw new IllegalArgumentException("invalid param");
        // 统计左右子树尺寸
        int left = count(target.left);
        int right = count(target.right);
        // 判断三部分是否有一部分大于一半，有则稳赢
        return left > n / 2 || right > n / 2 || left + right < n / 2;
    }

    private TreeNode findNode(TreeNode root, int x) {
        if (root == null) return null;
        if (root.val == x) return root;
        TreeNode left = findNode(root.left, x);
        if (left != null) return left;
        TreeNode right = findNode(root.right, x);
        return right;
    }

    private int count(TreeNode root) {
        if (root == null) return 0;
        return count(root.left) + count(root.right) + 1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为 $O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.7MB，在所有java提交中击败了15.38%的用户。

## 官方解题

&emsp;暂无，密切关注。