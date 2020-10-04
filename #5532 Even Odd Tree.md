[toc]

A binary tree is named **Even-Odd** if it meets the following conditions:

* The root of the binary tree is at level index $0$, its children are at level index $1$, their children are at level index $2$, etc.
* For every **even-indexed** level, all nodes at the level have **odd** integer values in **strictly increasing** order (from left to right).
* For every **odd-indexed** level, all nodes at the level have **even** integer values in **strictly decreasing** order (from left to right).

Given the `root` of a binary tree, return `true` if the binary tree is **Even-Odd**, otherwise return `false`.



**Constraints**:

* The number of nodes in the tree is in the range $[1, 10^5]$.
* $1 \le \text{Node.val} \le 10^6$



## 题目解读

&emsp;定义奇偶树，偶数层是奇数且严格递增；奇数层是偶数且严格递减。判断给定的树是否是奇偶树。

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
    public boolean isEvenOddTree(TreeNode root) {
        
    }
}
```

## 程序设计

* 二叉树层次遍历，并判断是否符合条件。

```java
class Solution {
    public boolean isEvenOddTree(TreeNode root) {
        if (root == null) return true;
        // 是否偶数层
        boolean evenLevel = true;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            // 记录前一个值
            int pre = evenLevel ? -1 : Integer.MAX_VALUE;
            for (int i = 0; i < size; i++) {
                TreeNode cur = queue.poll();
                // 偶数层为奇数且递增
                if (evenLevel && (cur.val % 2 == 0 || cur.val <= pre)) return false;
                // 奇数层为偶数且递减
                else if (!evenLevel && (cur.val % 2 == 1 || cur.val >= pre)) return false;
                
                pre = cur.val;
                if (cur.left != null) queue.add(cur.left);
                if (cur.right != null) queue.add(cur.right);
            }
            // 下一层迭代
            evenLevel = !evenLevel;
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。



## 官方解题

&emsp;