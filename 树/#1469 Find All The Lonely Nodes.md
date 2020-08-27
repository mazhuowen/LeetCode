[toc]

In a binary tree, a **lonely** node is a node that is the only child of its parent node. The root of the tree is not lonely because it does not have a parent node.

Given the `root` of a binary tree, return an array containing the values of all lonely nodes in the tree. Return the list **in any order**.



**Constraints**:

* The number of nodes in the tree is in the range $[1, 1000]$.
* Each node's value is between $[1, 10^6]$.



## 题目解读

&emsp;孤独节点定义为父节点的唯一子节点。返回所有的孤独节点。

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
    public List<Integer> getLonelyNodes(TreeNode root) {
        
    }
}
```

## 程序设计

* 后序遍历判断。

```java
class Solution {
    List<Integer> res;

    public List<Integer> getLonelyNodes(TreeNode root) {
        this.res = new LinkedList<>();

        getLonelyNode(root);
        return res;
    }

    private boolean getLonelyNode(TreeNode root) {
        if (root == null) return false;

        boolean left = getLonelyNode(root.left);
        boolean right = getLonelyNode(root.right);
        if (left && !right) res.add(root.left.val);
        if (!left && right) res.add(root.right.val);
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了99.45%的用户。

内存消耗：40.8MB，在所有java提交中击败了5.77%的用户。

## 官方解题

&emsp;暂无，密切关注。