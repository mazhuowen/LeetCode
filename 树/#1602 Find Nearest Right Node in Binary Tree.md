[toc]

Given the `root` of a binary tree and a node `u` in the tree, return the **nearest** node on the **same level** that is to the **right** of `u`, or return `null` if `u` is the rightmost node in its level.



**Constraints**:

* The number of nodes in the tree is in the range $[1, 10^5]$.
* $1 \le \text{Node.val} \le 10^5$
* All values in the tree are **distinct**.
* `u` is a node in the binary tree rooted at `root`.



## 题目解读

&emsp;返回节点同层左侧最近的节点。

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
    public TreeNode findNearestRightNode(TreeNode root, TreeNode u) {
        
    }
}
```

## 程序设计

* 层次遍历。

```java
class Solution {
    public TreeNode findNearestRightNode(TreeNode root, TreeNode u) {
        if (root == null || u == null) throw new IllegalArgumentException("invalid param");
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode cur = queue.poll();
                if (cur == u) {
                    return i == size - 1 ? null : queue.poll();
                }
                if (cur.left != null) queue.add(cur.left);
                if (cur.right != null) queue.add(cur.right);
            }
        }
        return null;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：14ms，在所有java提交中击败了92.31%的用户。

内存消耗：49.9MB，在所有java提交中击败了92.31%的用户。

## 官方解题

&emsp;暂无，密切关注。