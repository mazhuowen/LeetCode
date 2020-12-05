[toc]

Given a non-empty special binary tree consisting of nodes with the non-negative value, where each node in this tree has exactly `two` or `zero` sub-node. If the node has two sub-nodes, then this node's value is the smaller value among its two sub-nodes. More formally, the property `root.val = min(root.left.val, root.right.val)` always holds.

Given such a binary tree, you need to output the **second minimum** value in the set made of all the nodes' value in the whole tree.

If no such second minimum value exists, output $-1$ instead.

 

**Example 1**:

<img src="..\images\#671_exp1.jpg" style="zoom:80%;" />

```
Input: root = [2,2,5,null,null,5,7]
Output: 5
Explanation: The smallest value is 2, the second smallest value is 5.
```

**Example 2**:

<img src="..\images\#671_exp2.jpg" style="zoom:80%;" />

```
Input: root = [2,2,2]
Output: -1
Explanation: The smallest value is 2, but there isn't any second smallest value.
```



**Constraints**:

* The number of nodes in the tree is in the range $[1, 25]$.
* $1 \le \text{Node.val} \le 2^{31} - 1$
* `root.val == min(root.left.val, root.right.val)` for each internal node of the tree.



## 题目解读

&emsp;给定二叉树每个节点要么有两个子节点，要么没有子节点，当前节点值是两个子节点值的较小值；求次小值。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public int findSecondMinimumValue(TreeNode root) {

    }
}
```

## 程序设计

* 后序遍历二叉树，返回子树中的次小值，这样当前候选数值为：左右子树最小值（左右子节点值），左右子树次小值（左右子节点递归值），从这四个值中选择次小值；

```java
class Solution {
    public int findSecondMinimumValue(TreeNode root) {
        // 空节点或叶节点不存在第二小的值
        if (root == null || root.left == null) return -1;

        // 如果子节点值不一致，则预设节点值为较大值
        int res = root.left.val == root.right.val ? -1 : Math.max(root.left.val, root.right.val);

        if (root.left.val == root.val) {
            // 左子树次小值
            int left = findSecondMinimumValue(root.left);
            if (left != -1 && left > root.val && (left < res || res == -1)) res = left;
        }
        if (root.right.val == root.val) {
            // 右子树次小值
            int right = findSecondMinimumValue(root.right);
            if (right != -1 && right > root.val && (right < res || res == -1)) res = right;
        }

        return res;
    }
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户。

内存消耗：35.9 MB, 在所有 Java 提交中击败了61.54%的用户。

## 官方解题

&emsp;官方思路类似，实现更简洁明了。

```java
class Solution {
    public int findSecondMinimumValue(TreeNode root) {
        if (root == null) return -1;
        return findSecondMinimumValue(root, root.val);
    }

    // 返回比min大的最小值
    private int findSecondMinimumValue(TreeNode root, int min) {
        if (root == null) return -1;

        // 子树根节点比最小值大，返回
        if (root.val > min) return root.val;

        int left = findSecondMinimumValue(root.left, min);
        int right = findSecondMinimumValue(root.right, min);
        // 左子树不存在次小值
        if (left == -1) return right;
        // 右子树不存在次小值
        if (right == -1) return left;
        // 返回较小值
        return Math.min(left, right);
    }
}
```

