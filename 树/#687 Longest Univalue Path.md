[toc]

Given the `root` of a binary tree, return the length of the longest path, where each node in the path has the same value. This path may or may not pass through the root.

**The length of the path** between two nodes is represented by the number of edges between them.

 

**Example 1**:

<img src="..\images\#687_exp1.jpg" style="zoom:80%;" />

```
Input: root = [5,4,5,1,1,5]
Output: 2
```

**Example 2**:

<img src="..\images\#687_exp2.jpg" style="zoom:80%;" />

```
Input: root = [1,4,5,4,4,5]
Output: 2
```



**Constraints**:

* The number of nodes in the tree is in the range $[0, 10^4]$.
* $-1000 \le \text{Node.val} \le 1000$
* The depth of the tree will not exceed $1000$.



## 题目解读

&emsp;统计二叉树最长的节点值相等的路径。

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
    public int longestUnivaluePath(TreeNode root) {

    }
}
```

## 程序设计

* 后序遍历返回以当前节点为根的最长单链路径，判断左右子节点更新最长路径。

```java
class Solution {
    private int maxLen = 0;

    public int longestUnivaluePath(TreeNode root) {
        longestPathWithRoot(root);
        return maxLen;
    }

    private int longestPathWithRoot(TreeNode root) {
        if (root == null) return 0;
        int left = longestPathWithRoot(root.left);
        int right = longestPathWithRoot(root.right);
        // 更新当前路径
        int curLen = 0, path = 0;
        if (root.left != null && root.left.val == root.val) {
            curLen += left + 1;
            path = Math.max(path, left + 1);
        }
        if (root.right != null && root.right.val == root.val) {
            curLen += right + 1;
            path = Math.max(path, right + 1);
        }
        maxLen = Math.max(maxLen, curLen);
		// 返回最长单链路径
        return path;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：3 ms, 在所有 Java 提交中击败了84.53%的用户

内存消耗：41.8 MB, 在所有 Java 提交中击败了37.93%的用户。

## 官方解题

&emsp;官方思路类似。
