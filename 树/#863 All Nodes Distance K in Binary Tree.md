[toc]

We are given a binary tree (with root node `root`), a `target` node, and an integer value `K`.

Return a list of the values of all nodes that have a distance `K` from the `target` node.  The answer can be returned in any order.

Note:

* The given tree is non-empty.
* Each node in the tree has unique values $0 \le \text{node.val} \le 500$.
* The `target` node is a node in the tree.
* $0 \le K \le 1000$.



## 题目解读

&emsp;给定目标结点，给出距离该目标结点距离为`K`的所有结点。

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
    public List<Integer> distanceK(TreeNode root, TreeNode target, int K) {
        
    }
}
```

## 程序设计

* 对于目标结点距离为`K`的点，存在三种可能，一种是在目标点的子树中，一种是在目标点的祖先路径上。还有一种是在祖先路径的另一分支上。问题可以分解为自目标点回溯祖先结点，然后检查判断另一个分支。

```java
class Solution {
    List<Integer> res;

    public List<Integer> distanceK(TreeNode root, TreeNode target, int K) {
        res = new LinkedList<>();
        if (root == null || target == null || K < 0) return res;
        findTargetPath(root, target, K);
        return res;
    }

    // 查找目标点祖先路径
    private int findTargetPath(TreeNode root, TreeNode target, int K) {
        if (root == null) return -1;
        if (root == target) {
            findNextKTh(root, K);
            return 1;
        }
        int dis = findTargetPath(root.left, target, K);
        // 左子树包含target
        if (dis != -1) {
            if (dis == K) res.add(root.val);
            // 在右子树查找，注意减一
            else findNextKTh(root.right, K - dis - 1);
            return dis + 1;
        }
        // 右子树包含target
        dis = findTargetPath(root.right, target, K);
        if (dis != -1) {
            if (dis == K) res.add(root.val);
            // 在左子树查找，注意减一
            else findNextKTh(root.left, K - dis - 1);
            return dis + 1;
        }
        // 左右都不包含target
        return -1;
    } 

    // 查找距离根为K的子节点
    private void findNextKTh(TreeNode root, int K) {
        if (root == null || K < 0) return;
        // 找到
        if (K == 0) {
            res.add(root.val);
            return;
        }
        findNextKTh(root.left, K - 1);
        findNextKTh(root.right, K - 1);
    }
}
```

## 性能分析

&emsp;由于遍历两遍树，时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：13ms，在所有java提交中击败了95.69%的用户。

内存消耗：39.9MB，在所有java提交中击败了12.50%的用户。

## 官方解题

&emsp;官方还提供了遍历时计算结点间距离的思路。