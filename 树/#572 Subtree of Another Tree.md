[toc]

Given two non-empty binary trees s and t, check whether tree t has exactly the same structure and node values with a subtree of s. A subtree of s is a tree consists of a node in s and all of this node's descendants. The tree s could also be considered as a subtree of itself.



## 题目解读

&emsp;检查一棵树是否是另一棵树的子结构。

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
    public boolean isSubtree(TreeNode s, TreeNode t) {
        
    }
}
```

## 程序设计

* 首先遍历查找比较根节点值是否一致，然后比较两棵树是否一致。拆分为两步。

```java
class Solution {
    public boolean isSubtree(TreeNode s, TreeNode t) {
        if (t == null) return true;
        if (s == null) return t == null;

        if (s.val == t.val && isSame(s, t)) return true;

        return isSubtree(s.left, t) || isSubtree(s.right, t);
    }

    private boolean isSame(TreeNode s, TreeNode t) {
        if (s == null) return t == null;
        if (t == null || s.val != t.val) return false;
        return isSame(s.left, t.left) && isSame(s.right, t.right);
    }
}
```

## 性能分析

&emsp;最坏情况时间复杂度为$O(MN)$，空间复杂度为$O(\log_2M)$，最坏$O(M)$。

执行用时：7ms，在所有java提交中击败了93.35%的用户。

内存消耗：40MB，在所有java提交中击败了60.00%的用户。

## 官方解题

&emsp;同上。